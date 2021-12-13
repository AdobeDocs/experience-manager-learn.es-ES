---
title: Conexiones SQL con API de Java™
description: Obtenga información sobre cómo conectar con bases de datos SQL desde AEM as a Cloud Service mediante las API SQL de Java™ y los puertos de salida.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9356
thumbnail: KT-9356.jpeg
exl-id: ec9d37cb-70b6-4414-a92b-3b84b3f458ab
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 0%

---

# Conexiones SQL con API de Java™

Las conexiones a bases de datos SQL (y otros servicios no HTTP/HTTPS) deben procesarse como proxy fuera de AEM.

La excepción a esta regla es cuando [dirección ip de salida dedicada](../dedicated-egress-ip-address.md) está en uso y el servicio está en Adobe o Azure.

## Compatibilidad avanzada con redes

El siguiente ejemplo de código es compatible con las siguientes opciones avanzadas de red.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ü | š | š | š |

## Configuración de OSGi

Dado que los secretos no deben almacenarse en el código, el nombre de usuario y la contraseña de la conexión SQL se proporcionan mejor mediante [variables de configuración OSGi secretas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), se configura mediante la CLI de AIO o las API de Cloud Manager.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/com.adobe.aem.wknd.examples.core.connections.impl.MySqlExternalServiceImpl.cfg.json`

```json
{
  "username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "password": "$[secret:MYSQL_PASSWORD]"
}
```

Lo siguiente `aio CLI` puede utilizarse para configurar los secretos OSGi por entorno:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Ejemplo de código

Este ejemplo de código Java™ es de un servicio OSGi que realiza una conexión a un servidor web SQL Server externo a través de Cloud Manager `portForwards` regla de [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/MySqlExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.AttributeType;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.sql.*;

@Component
@Designate(ocd = MySqlExternalServiceImpl.Config.class)
public class MySqlExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(MySqlExternalServiceImpl.class);

    // Get the proxy host using the AEM_PROXY_HOST Java environment variable provided by AEM as a Cloud Service
    private static final String PROXY_HOST = System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel");

    // Use the port mapped to the external MySQL service in the Cloud Manager API call
    private static final int PORT_FORWARDS_PORT_ORIG = 30001;

    private Config config;

    @Override
    public boolean isAccessible() {
        log.debug("MySQL connection URL: [ {} ]", getConnectionUrl(config));

        // Establish a connection with the external MySQL service
        // This MySQL connection URL is created in getConnection(..) which will use the AEM_PROXY_HOST is it exists, and the proxied port.
        try (Connection connection = DriverManager.getConnection(getConnectionUrl(config), config.username(), config.password())) {
            // Validate the connection
            connection.isValid(1000);

            // Close the connection, since this is just a simple connectivity check
            connection.close();

            // Return true if AEM could reach the external SQL service
            return true;
        } catch (SQLException e) {
            log.error("Unable to establish an connection with MySQL service using connection URL  [ {} ]", getConnectionUrl(config), e);
        }

        return false;
    }

    /**
     * Create a connection string to the MySQL using the AEM-provided AEM_PROXY_HOST and portForwards.portOrg port
     * defined in the Cloud Manager API mapping.
     *
     * @param config OSGi configuration object
     * @return the MySQL connection URI
     */
    private String getConnectionUrl(Config config) {
        return String.format("jdbc:mysql://%s:%d/wknd-examples", PROXY_HOST, PORT_FORWARDS_PORT_ORIG);
    }

    @Activate
    protected void activate(Config config) throws ClassNotFoundException, SQLException {
        this.config = config;

        // Load the required MySQL Driver class required for Java to make the connection
        // The OSGi bundle that contains this driver is deployed via the project's all project
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
    }

    @ObjectClassDefinition
    @interface Config {
        @AttributeDefinition(type = AttributeType.STRING)
        String username();

        @AttributeDefinition(type = AttributeType.STRING)
        String password();
    }
}
```

## Dependencias del controlador MySQL

AEM as a Cloud Service a menudo requiere que proporcione controladores de base de datos Java™ para admitir las conexiones. La mejor manera de conseguir los controladores es incrustando los artefactos del paquete OSGi que contienen estos controladores en el proyecto AEM a través de la variable `all` paquete.

### Reactor pom.xml

Incluir las dependencias de controlador de base de datos en el reactor `pom.xml` y luego haga referencia a en la variable `all` subproyectos.

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## Todo pom.xml

Incruste los artefactos de dependencia del controlador de la base de datos en la variable `all` a se implementan y están disponibles en AEM as a Cloud Service. Estos artefactos __must__ sean paquetes OSGi que exporten la clase Java™ del controlador de base de datos.

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
