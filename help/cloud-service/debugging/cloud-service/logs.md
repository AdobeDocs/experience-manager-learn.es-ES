---
title: Registros
description: AEM AEM Los registros actúan como primera línea para depurar aplicaciones de la en el as a Cloud Service AEM, pero dependen del registro adecuado en la aplicación de la aplicación de la aplicación implementada de la.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 229
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# AEM Depuración de registros as a Cloud Service con

AEM AEM Los registros actúan como primera línea para depurar aplicaciones de la en el as a Cloud Service AEM, pero dependen del registro adecuado en la aplicación de la aplicación de la aplicación implementada de la.

AEM Toda la actividad de registro del servicio de registro de un entorno determinado (Dispatcher de creación, publicación o publicación) se consolida en un único archivo de registro, incluso si diferentes pods dentro de ese servicio generan las instrucciones de registro.

Los ID de secuencia se proporcionan en cada sentencia de registro y permiten filtrar o intercalar sentencias de registro. Los ID de la secuencia tienen el formato:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Ejemplo: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Archivos de registro personalizados

AEM como Cloud Service no admite archivos de registro personalizados, pero sí admite el registro personalizado.

AEM Para que los registros de Java estén disponibles en el as a Cloud Service (a través de ). [Cloud Manager](#cloud-manager) o [CLI DE ADOBE I/O](#aio)), las instrucciones de registro personalizadas deben escribirse en `error.log`. Registros escritos en registros con nombres personalizados, como `example.log`AEM , no será accesible desde el as a Cloud Service de la.

Los registros se pueden escribir en `error.log` usar una propiedad de configuración OSGi de LogManager de Sling en el `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` archivos.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM Registros del servicio de autor y publicación de

AEM AEM Tanto los servicios de autor como los de publicación proporcionan registros de servidor en tiempo de ejecución de la:

+ `aemerror` es el registro de errores de Java (encontrado en `/crx-quickstart/logs/error.log` AEM en el inicio rápido local del SDK de la). Los siguientes son los [niveles de registro recomendados](#log-levels) para registradores personalizados por tipo de entorno:
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`
+ `aemaccess` AEM enumera las solicitudes HTTP al servicio de con detalles
+ `aemrequest` AEM enumera las solicitudes HTTP realizadas al servicio de y su respuesta HTTP correspondiente

## AEM Publicar registros de Dispatcher

AEM AEM AEM Solo Dispatcher de publicación de datos proporciona registros de Apache Web Server y Dispatcher, ya que estos aspectos solo existen en el nivel de publicación de la y no en el nivel de Author de la.

+ `httpdaccess` AEM enumera las solicitudes HTTP realizadas al servidor web Apache/Dispatcher del servicio de.
+ `httperror`  enumera los mensajes de registro del servidor web Apache y ayuda con la depuración de los módulos Apache admitidos, como `mod_rewrite`.
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`
+ `aemdispatcher` enumera los mensajes de registro de los módulos de Dispatcher, incluidos los mensajes de filtrado y envío desde la caché.
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager permite la descarga de registros, por día, a través de la acción Descargar registros de un entorno.

![Cloud Manager: registros de descarga](./assets/logs/download-logs.png)

Estos registros se pueden descargar e inspeccionar mediante cualquier herramienta de análisis de registros.

## CLI de Adobe I/O con complemento de Cloud Manager{#aio}

Adobe AEM Cloud Manager admite el acceso a los registros as a Cloud Service de la a través de [CLI DE ADOBE I/O](https://github.com/adobe/aio-cli) con el [Complemento de Cloud Manager para la CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Primero, [configuración del complemento Adobe I/O con Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Asegúrese de que se han identificado el ID de programa y el ID de entorno relevantes y utilice [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) para enumerar las opciones de registro utilizadas para lo siguiente [cola](#aio-cli-tail-logs) o [descargar](#aio-cli-download-logs) registros.

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### Registros de cola{#aio-cli-tail-logs}

La CLI de Adobe I/O AEM proporciona la capacidad de rastrear registros en tiempo real desde la as a Cloud Service con el uso de la función de seguimiento en tiempo real de. [registros de cola](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) comando. AEM El seguimiento es útil para ver la actividad de registro en tiempo real a medida que las acciones se realizan en el entorno as a Cloud Service de la.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Otras herramientas de línea de comandos, como `grep` se puede usar de forma conjunta con `tail-logs` para ayudar a aislar las instrucciones de registro de interés, por ejemplo:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... solo muestra las instrucciones de registro generadas a partir de `com.example.MySlingModel` o contener esa cadena en ellos.

### Descarga de registros{#aio-cli-download-logs}

La CLI de Adobe I/O AEM proporciona la capacidad de descargar registros de los registros as a Cloud Service mediante el uso de la función de descarga de registros de. [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)), comando. Esto proporciona el mismo resultado final que descargar los registros de la interfaz de usuario web de Cloud Manager, con la diferencia de que `download-logs` El comando consolida los registros a lo largo de los días, en función de cuántos días se soliciten.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Explicación de registros

AEM Los inicios de sesión as a Cloud Service tienen varios pods que escriben instrucciones de registro en ellos. AEM Dado que varias instancias de escriben en el mismo archivo de registro, es importante comprender cómo analizar y reducir el ruido durante la depuración. Para explicar, lo siguiente `aemerror` se utiliza un fragmento de registro:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

AEM Mediante los ID de secuencia, el punto de datos después de la fecha y la hora, los registros se pueden recopilar mediante Pod o la instancia de dentro del servicio, lo que facilita el seguimiento y la comprensión de la ejecución del código.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Niveles de registro recomendados{#log-levels}

Las directrices generales del Adobe AEM sobre los niveles de registro según el entorno as a Cloud Service de la son las siguientes:

+ AEM Desarrollo local (SDK de): `DEBUG`
+ Desarrollo: `DEBUG`
+ Escenario: `WARN`
+ Producción: `ERROR`

La configuración del nivel de registro más adecuado para cada tipo de entorno se realiza con la configuración as a Cloud Service, los niveles de registro se mantienen en el código, lo que hace que los valores de registro sean los más AEM

+ Las configuraciones de registro de Java se mantienen en las configuraciones de OSGi
+ Niveles de registro del servidor web Apache y Dispatcher en el proyecto de Dispatcher

...y por lo tanto, requieren una implementación para cambiar.

### Variables específicas del entorno para establecer los niveles de registro de Java

AEM Una alternativa a establecer niveles de registro de Java bien conocidos y estáticos para cada entorno es usar la opción de los Cloud Service como de la aplicación de la aplicación de la manera más sencilla. [variables específicas del entorno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) para parametrizar los niveles de registro, permitiendo que los valores se cambien dinámicamente mediante el [CLI de Adobe I/O con complemento de Cloud Manager](#aio-cli).

Esto requiere actualizar las configuraciones de registro de OSGi para utilizar los marcadores de posición de variables específicos del entorno. [Valores predeterminados](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) para los niveles de registro deben establecerse de acuerdo con [Adobe de recomendaciones](#log-levels). Por ejemplo:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Este enfoque tiene desventajas que deben tenerse en cuenta:

+ [Se permite un número limitado de variables de entorno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)y al crear una variable para administrar el nivel de registro se utilizará uno.
+ Las variables de entorno se pueden administrar mediante programación mediante [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), [CLI DE ADOBE I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid), y [API HTTP de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ Los cambios en las variables de entorno deben restablecerse manualmente mediante una herramienta compatible. AEM El olvido de restablecer un entorno de alto tráfico, como Producción, a un nivel de registro menos detallado puede inundar los registros e afectar al rendimiento de la.

_Las variables específicas del entorno no funcionan para las configuraciones de registro del servidor web Apache o Dispatcher, ya que no se configuran mediante la configuración OSGi._
