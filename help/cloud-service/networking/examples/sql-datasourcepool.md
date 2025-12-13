---
title: Conexiones SQL con el conjunto de fuentes de datos JDBC
description: Obtenga información sobre cómo conectarse a bases de datos SQL desde AEM as a Cloud Service mediante el grupo de fuentes de datos JDBC y los puertos de salida de AEM.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
duration: 117
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 0%

---

# Conexiones SQL con el conjunto de fuentes de datos JDBC

Las conexiones a bases de datos SQL (y otros servicios no HTTP/HTTPS) deben procesarse como proxy fuera de AEM, incluidos los que se realizan mediante el servicio OSGi DataSourcePool de AEM para administrar las conexiones.

## Compatibilidad avanzada con redes

Las siguientes opciones avanzadas de red admiten el siguiente ejemplo de código.

Asegúrese de que la configuración avanzada de red [proper](../advanced-networking.md#advanced-networking) se haya configurado antes de seguir este tutorial.

| Sin redes avanzadas | [Salida de puerto flexible](../flexible-port-egress.md) | [Dirección IP de salida dedicada](../dedicated-egress-ip-address.md) | [Red privada virtual](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuración de OSGi

La cadena de conexión de la configuración OSGi utiliza:

+ Valor de `AEM_PROXY_HOST` a través de la variable de entorno de configuración [OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=en#environment-specific-configuration-values) `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` como host de la conexión
+ `30001`, que es el valor `portOrig` para la asignación de reenvío de puerto de Cloud Manager `30001` → `mysql.example.com:3306`

Dado que los secretos no deben almacenarse en el código, el nombre de usuario y la contraseña de la conexión SQL se proporcionan mejor mediante variables de configuración OSGi, establecidas mediante CLI de AIO o API de Cloud Manager.

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

El siguiente comando `aio CLI` se puede usar para establecer los secretos OSGi por entorno:

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## Ejemplo de código

Este ejemplo de código Java™ es de un servicio OSGi que realiza una conexión con una base de datos MySQL externa a través del servicio OSGi de DataSourcePool de AEM.
A su vez, la configuración de fábrica de DataSourcePool OSGi especifica un puerto (`30001`) que se asigna a través de la regla `portForwards` en la operación [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) al host y al puerto externos, `mysql.example.com:3306`.

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

AEM as a Cloud Service suele requerir que proporcione controladores de base de datos Java™ para admitir las conexiones. La mejor manera de proporcionar los controladores suele ser incrustar los artefactos del paquete OSGi que contienen estos controladores en el proyecto de AEM a través del paquete `all`.

### Reactor pom.xml

Incluya las dependencias del controlador de base de datos en el reactor `pom.xml` y haga referencia a ellas en los subproyectos `all`.

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

Incruste los artefactos de dependencia del controlador de base de datos en el paquete `all` para que se implementen y estén disponibles en AEM as a Cloud Service. Estos artefactos __deben__ ser paquetes OSGi que exportan la clase Java™ del controlador de base de datos.

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
