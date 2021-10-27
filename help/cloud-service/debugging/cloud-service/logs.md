---
title: Registros
description: Los registros actúan como primera línea para depurar AEM aplicaciones en AEM as a Cloud Service, pero dependen del registro adecuado en la aplicación de AEM implementada.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
source-git-commit: eb669d1e2493d9b4a973314ab1323764920ba220
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 3%

---

# Depuración AEM as a Cloud Service mediante registros

Los registros actúan como primera línea para depurar AEM aplicaciones en AEM as a Cloud Service, pero dependen del registro adecuado en la aplicación de AEM implementada.

Toda la actividad de registro de un servicio de AEM de un entorno determinado (Autor, Publicar/Publicar Dispatcher) se consolida en un solo archivo de registro, incluso si distintos pods dentro de ese servicio generan las sentencias de registro.

Los identificadores de la secuencia se proporcionan en cada instrucción de registro y permiten filtrar o cotejar las instrucciones de registro. Los identificadores de la secuencia tienen el formato siguiente:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Ejemplo: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Archivos de registro personalizados

AEM como Cloud Services no admite archivos de registro personalizados, pero sí admite este tipo de registro.

Para que los registros de Java estén disponibles en AEM as a Cloud Service (a través de [Cloud Manager](#cloud-manager) o [CLI de Adobe I/O](#aio)), las instrucciones de registro personalizadas deben escribirse en la variable `error.log`. Registros escritos en registros con nombres personalizados, como `example.log`, no se podrá acceder desde AEM as a Cloud Service.

Los registros se pueden escribir en el `error.log` uso de una propiedad de configuración OSGi de Sling LogManager en el `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` archivos.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Registros de servicio de AEM Author y Publish

Tanto AEM Author como Publish proporcionan registros AEM servidor de tiempo de ejecución:

+ `aemerror` es el registro de errores de Java (se encuentra en `/crx-quickstart/logs/error.log` en el inicio rápido local del SDK de AEM). Los siguientes son los [niveles de registro recomendados](#log-levels) para registradores personalizados por tipo de entorno:
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`
+ `aemaccess` enumera las solicitudes HTTP al servicio AEM con detalles
+ `aemrequest` enumera las solicitudes HTTP realizadas a AEM servicio y su respuesta HTTP correspondiente

## Registros de Dispatcher de AEM Publish

Solo AEM Publish Dispatcher proporciona registros del servidor web Apache y del Dispatcher, ya que estos aspectos solo existen en el nivel de AEM Publish y no en el de AEM Author.

+ `httpdaccess` enumera las solicitudes HTTP realizadas al servidor web Apache/Dispatcher del servicio AEM.
+ `httperror`  enumera los mensajes de registro del servidor web Apache y ayuda con la depuración de módulos Apache compatibles, como `mod_rewrite`.
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`
+ `aemdispatcher` enumera los mensajes de registro de los módulos de Dispatcher, incluido el filtrado y el servicio de mensajes de caché.
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`

## Cloud Manager{#cloud-manager}

Adobe Cloud Manager permite la descarga de registros, por día, a través de la acción Download Logs de un entorno.

![Cloud Manager: registros de descarga](./assets/logs/download-logs.png)

Estos registros se pueden descargar e inspeccionar mediante cualquier herramienta de análisis de registros.

## CLI de Adobe I/O con complemento de Cloud Manager{#aio}

Adobe Cloud Manager admite el acceso a AEM registros as a Cloud Service a través de [CLI de Adobe I/O](https://github.com/adobe/aio-cli) con la variable [Complemento de Cloud Manager para la CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Primero, [configuración del Adobe I/O con el complemento de Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Asegúrese de que se hayan identificado el ID de programa y el ID de entorno correspondientes y utilice [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) para enumerar las opciones de registro que se utilizan para [cola](#aio-cli-tail-logs) o [descargar](#aio-cli-download-logs) registros.

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

### Registros de seguimiento{#aio-cli-tail-logs}

La CLI de Adobe I/O proporciona la capacidad de rastrear registros en tiempo real desde AEM as a Cloud Service usando la [colas de registro](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) comando. La navegación es útil para ver la actividad de registro en tiempo real, ya que las acciones se realizan en el entorno as a Cloud Service AEM.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Otras herramientas de la línea de comandos, como `grep` se puede usar de forma conjunta con `tail-logs` para ayudar a aislar las declaraciones de interés de registro, por ejemplo:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... solo muestra las instrucciones de registro generadas a partir de `com.example.MySlingModel` o contener esa cadena en ellos.

### Registros de descarga{#aio-cli-download-logs}

Adobe I/O CLI proporciona la capacidad de descargar registros de AEM as a Cloud Service mediante el [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Esto proporciona el mismo resultado final que descargar los registros de la interfaz de usuario web de Cloud Manager, siendo la diferencia `download-logs` consolida los registros entre días, según la cantidad de días de registros solicitados.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Explicación de los registros

Los registros AEM as a Cloud Service tienen varios pods escribiendo instrucciones de registro en ellos. Dado que varias instancias de AEM escriben en el mismo archivo de registro, es importante comprender cómo analizar y reducir el ruido durante la depuración. Para explicar, haga lo siguiente: `aemerror` se utilizará el fragmento de registro:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Con los ID de la secuencia, el punto de datos después de la fecha y la hora, los registros se pueden recopilar mediante Pod o AEM instancia dentro del servicio, lo que facilita el seguimiento y comprensión de la ejecución del código.

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

Las directrices generales del Adobe sobre los niveles de registro por AEM entorno as a Cloud Service son:

+ Desarrollo local (AEM SDK): `DEBUG`
+ Desarrollo: `DEBUG`
+ Escenario: `WARN`
+ Producción: `ERROR`

La configuración del nivel de registro más adecuado para cada tipo de entorno es con AEM as a Cloud Service, los niveles de registro se mantienen en el código

+ Las configuraciones de registro Java se mantienen en configuraciones OSGi
+ Niveles de registro del servidor web Apache y Dispatcher en el proyecto de Dispatcher

...y, por lo tanto, requieren un despliegue para cambiar.

### Variables específicas del entorno para establecer niveles de registro de Java

Una alternativa a establecer niveles de registro de Java estáticos y conocidos para cada entorno es usar AEM como Cloud Service [variables específicas del entorno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) para parametrizar los niveles de registro, permitiendo que los valores se cambien dinámicamente a través de la variable [CLI de Adobe I/O con complemento de Cloud Manager](#aio-cli).

Esto requiere actualizar las configuraciones OSGi de registro para utilizar los marcadores de posición de variables específicas del entorno. [Valores predeterminados](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) para los niveles de registro, debe establecerse como [Recomendaciones de Adobe](#log-levels). Por ejemplo:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Este enfoque presenta aspectos negativos que deben tenerse en cuenta:

+ [Se permite un número limitado de variables de entorno](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables), y crear una variable para administrar el nivel de registro utilizará una.
+ Las variables de entorno solo se pueden administrar mediante programación mediante [CLI de Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) o [API HTTP de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ Las herramientas compatibles deben restablecer manualmente los cambios en las variables de entorno. Olvidar restablecer un entorno de tráfico alto, como Producción, a un nivel de registro menos detallado puede inundar los registros e influir en el rendimiento AEM.

_Las variables específicas del entorno no funcionan para las configuraciones de servidor web Apache o de registro de Dispatcher, ya que no se configuran mediante la configuración OSGi._
