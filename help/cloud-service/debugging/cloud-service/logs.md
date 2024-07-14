---
title: Registros
description: AEM Los registros actúan como primera línea para la depuración de aplicaciones de la en AEM as a Cloud Service AEM, pero dependen del registro adecuado en la aplicación de la aplicación de la aplicación implementada de la.
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

# Depuración de AEM as a Cloud Service mediante registros

AEM Los registros actúan como primera línea para la depuración de aplicaciones de la en AEM as a Cloud Service AEM, pero dependen del registro adecuado en la aplicación de la aplicación de la aplicación implementada de la.

AEM Toda la actividad de registro del servicio de registro de un entorno determinado (autor, Publish/Publish Dispatcher) se consolida en un único archivo de registro, aunque distintos pods dentro de ese servicio generen las sentencias de registro.

Los ID de secuencia se proporcionan en cada sentencia de registro y permiten filtrar o intercalar sentencias de registro. Los ID de la secuencia tienen el formato:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Ejemplo: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Archivos de registro personalizados

AEM como Cloud Service no admite archivos de registro personalizados, pero sí admite el registro personalizado.

Para que los registros de Java estén disponibles en AEM as a Cloud Service (a través de [Cloud Manager](#cloud-manager) o [CLI de Adobe I/O](#aio)), las instrucciones de registro personalizadas deben escribirse en `error.log`. No se podrá obtener acceso a los registros escritos en registros con nombres personalizados, como `example.log`, desde AEM as a Cloud Service.

Los registros se pueden escribir en `error.log` mediante una propiedad de configuración OSGi de LogManager de Sling en los archivos `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` de la aplicación.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## AEM Registros de servicio de Autor y Publish de

AEM Tanto los servicios de Autor como los de Publish AEM proporcionan registros de servidor de tiempo de ejecución de la:

+ AEM `aemerror` es el registro de errores de Java (que se encuentra en `/crx-quickstart/logs/error.log` en el inicio rápido local del SDK de la). Los siguientes son los [niveles de registro recomendados](#log-levels) para registradores personalizados por tipo de entorno:
   + Desarrollo: `DEBUG`
   + Fase: `WARN`
   + Producción: `ERROR`
+ AEM `aemaccess` enumera las solicitudes HTTP al servicio de con detalles
+ AEM `aemrequest` enumera las solicitudes HTTP realizadas al servicio de y su respuesta HTTP correspondiente

## AEM Registros de Dispatcher de Publish

AEM Solo Publish Dispatcher proporciona registros de Dispatcher AEM y del servidor web Apache, ya que estos aspectos solo existen en el nivel de Publish AEM de la y no en el nivel de Author de la.

+ AEM `httpdaccess` enumera las solicitudes HTTP realizadas al servidor web Apache/Dispatcher del servicio de la.
+ `httperror` enumera los mensajes de registro del servidor web Apache y ayuda para depurar los módulos Apache admitidos, como `mod_rewrite`.
   + Desarrollo: `DEBUG`
   + Fase: `WARN`
   + Producción: `ERROR`
+ `aemdispatcher` enumera los mensajes de registro de los módulos de Dispatcher, incluidos los mensajes de filtrado y servicio de la caché.
   + Desarrollo: `DEBUG`
   + Fase: `WARN`
   + Producción: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager permite descargar registros, por día, a través de la acción Descargar registros de un entorno.

![Cloud Manager - Descargar registros](./assets/logs/download-logs.png)

Estos registros se pueden descargar e inspeccionar mediante cualquier herramienta de análisis de registros.

## CLI de Adobe I/O con complemento de Cloud Manager{#aio}

Adobe Cloud Manager admite el acceso a los registros de AEM as a Cloud Service a través de [CLI de Adobe I/O](https://github.com/adobe/aio-cli) con el complemento [Cloud Manager para CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Primero [configure el Adobe I/O con el complemento Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Asegúrese de que se hayan identificado el identificador de programa y el identificador de entorno relevantes y use [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) para enumerar las opciones de registro que se usan en los registros [tail](#aio-cli-tail-logs) o [download](#aio-cli-download-logs).

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

La CLI de Adobe I/O proporciona la capacidad de rastrear registros en tiempo real desde AEM as a Cloud Service mediante el comando [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name). El seguimiento es útil para ver la actividad de registro en tiempo real a medida que las acciones se realizan en el entorno de AEM as a Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Otras herramientas de línea de comandos, como `grep`, se pueden usar junto con `tail-logs` para ayudar a aislar las instrucciones de registro de interés, por ejemplo:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... solo muestra las instrucciones de registro generadas a partir de `com.example.MySlingModel` o que contienen esa cadena.

### Descarga de registros{#aio-cli-download-logs}

La CLI de Adobe I/O proporciona la capacidad de descargar registros de AEM as a Cloud Service mediante el comando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Esto proporciona el mismo resultado final que descargar los registros de la interfaz de usuario web de Cloud Manager, con la diferencia de que el comando `download-logs` consolida los registros a lo largo de los días, en función de cuántos días se soliciten.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Explicación de registros

Los registros de AEM as a Cloud Service tienen varios pods que escriben instrucciones de registro en ellos. AEM Dado que varias instancias de escriben en el mismo archivo de registro, es importante comprender cómo analizar y reducir el ruido durante la depuración. Para explicarlo, se usa el siguiente fragmento de registro `aemerror`:

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

Las directrices generales del Adobe sobre los niveles de registro por entorno de AEM as a Cloud Service son las siguientes:

+ AEM Desarrollo local (SDK de la): `DEBUG`
+ Desarrollo: `DEBUG`
+ Fase: `WARN`
+ Producción: `ERROR`

La configuración del nivel de registro más adecuado para cada tipo de entorno se realiza con AEM as a Cloud Service, los niveles de registro se mantienen en el código

+ Las configuraciones de registro de Java se mantienen en las configuraciones de OSGi
+ Niveles de registro del servidor web Apache y de Dispatcher en el proyecto de Dispatcher

...y por lo tanto, requieren una implementación para cambiar.

### Variables específicas del entorno para establecer los niveles de registro de Java

AEM Una alternativa a establecer niveles de registro de Java estáticos bien conocidos para cada entorno es usar como [variables específicas del entorno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) de los Cloud Service para parametrizar los niveles de registro, permitiendo que los valores se cambien dinámicamente a través de la [CLI de Adobe I/O con el complemento de Cloud Manager](#aio-cli).

Esto requiere actualizar las configuraciones de registro de OSGi para utilizar los marcadores de posición de variables específicos del entorno. [Los valores predeterminados](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) de los niveles de registro deben establecerse de acuerdo con las [recomendaciones de Adobe](#log-levels). Por ejemplo:

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

+ [Se permite un número limitado de variables de entorno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables), y al crear una variable para administrar el nivel de registro se utilizará una.
+ Las variables de entorno se pueden administrar mediante programación a través de [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), [CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) y [API HTTP de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ Los cambios en las variables de entorno deben restablecerse manualmente mediante una herramienta compatible. AEM El olvido de restablecer un entorno de alto tráfico, como el de Producción, a un nivel de registro menos detallado puede inundar los registros e afectar al rendimiento de los.

_Las variables específicas del entorno no funcionan para las configuraciones de registro del servidor web Apache o Dispatcher, ya que no se configuran mediante la configuración OSGi._
