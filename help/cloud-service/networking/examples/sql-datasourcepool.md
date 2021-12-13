---
title: Conexiones SQL usando DataSourcePool de JDBC
description: Obtenga información sobre cómo conectar con bases de datos SQL desde AEM as a Cloud Service mediante AEM puertos de salida y DataSourcePool de JDBC.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
source-git-commit: 6ed26e5c9bf8f5e6473961f667f9638e39d1ab0e
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 0%

---

# Conexiones SQL usando DataSourcePool de JDBC

Las conexiones a bases de datos SQL (y otros servicios que no sean HTTP/HTTPS) deben procesarse como proxy fuera de AEM, incluidos los realizados mediante AEM servicio OSGi DataSourcePool para administrar las conexiones.

## Compatibilidad avanzada con redes

El siguiente ejemplo de código es compatible con las siguientes opciones avanzadas de red.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ü | š | š | š |

## Configuración de OSGi

La cadena de conexión de la configuración OSGi utiliza:

+ `AEM_PROXY_HOST` a través de la variable [Variable de entorno de configuración OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` como host de la conexión
+ `30001` que es el `portOrig` valor para la asignación de reenvío de puerto de Cloud Manager `30001` → `mysql.example.com:3306`

Dado que los secretos no deben almacenarse en código, el nombre de usuario y la contraseña de la conexión SQL se proporcionan mejor a través de variables de configuración OSGi, configuradas mediante la CLI de AIO o las API de Cloud Manager.

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST;default=proxy.tunnel]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

Lo siguiente `aio CLI` puede utilizarse para configurar los secretos OSGi por entorno:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Ejemplo de código

Este ejemplo de código Java™ es de un servicio OSGi que realiza una conexión a una base de datos MySQL externa a través AEM servicio OSGi DataSourcePool.
La configuración de fábrica de DataSourcePool OSGi a su vez especifica un puerto (`30001`) que se asigna a través de la variable `portForwards` en el [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) operación al host y puerto externos, `mysql.example.com:3306`.

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
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
