---
title: Registros
description: Los registros actúan como primera línea para depurar aplicaciones AEM en AEM como Cloud Service, pero dependen del registro adecuado en la aplicación AEM implementada.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5432
thumbnail: kt-5432.jpg
translation-type: tm+mt
source-git-commit: 1eb15af3d9d2904856218aaad4d5c52233603a71
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 3%

---


# Depuración de AEM como Cloud Service mediante registros

Los registros actúan como primera línea para depurar aplicaciones AEM en AEM como Cloud Service, pero dependen del registro adecuado en la aplicación AEM implementada.

Toda la actividad de registro de un servicio de AEM de un entorno determinado (Autor, Publicar/Publicar despachante) se consolida en un solo archivo de registro, incluso si los distintos pods de ese servicio generan las sentencias de registro.

Los identificadores de pod se proporcionan en cada instrucción de registro y permiten filtrar o cotejar las sentencias de registro. Los ID de pod tienen el formato:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Ejemplo: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Registros de servicio de AEM Author y Publish

Los servicios AEM Author y Publish proporcionan registros AEM del servidor en tiempo de ejecución:

+ `aemerror` es el registro de errores de Java (se encuentra en `/crx-quickstart/error.log` el inicio rápido local del SDK de AEM). Los siguientes son los niveles [de registro](#log-levels) recomendados para los registradores personalizados por tipo de entorno:
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`
+ `aemaccess` lista solicitudes HTTP al servicio de AEM con detalles
+ `aemrequest` listas de solicitudes HTTP realizadas al servicio AEM y su correspondiente respuesta HTTP

## Registros de despachante de AEM Publish

Solo AEM Publish Dispatcher proporciona los registros de Dispatcher y del servidor web Apache, ya que estos aspectos solo existen en el nivel de AEM Publish y no en el de AEM Author.

+ `httpdaccess` lista las solicitudes HTTP realizadas al servidor web Apache/Dispatcher del servicio de AEM.
+ `httperror`  listas registran mensajes del servidor web Apache y ayuda con la depuración de módulos Apache compatibles como `mod_rewrite`.
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`
+ `aemdispatcher` Mensajes de registro de listas de los módulos Dispatcher, incluido el filtrado y el servicio desde mensajes de caché.
   + Desarrollo: `DEBUG`
   + Escenario: `WARN`
   + Producción: `ERROR`

## Cloud Manager

Adobe Cloud Manager permite la descarga de registros, por día, a través de la acción Descargar registros de un entorno.

![Cloud Manager: registros de descarga](./assets/logs/download-logs.png)

Estos registros pueden descargarse e inspeccionarse mediante cualquier herramienta de análisis de registros.

## CLI de E/S de Adobe con complemento Cloud Manager

Adobe Cloud Manager admite el acceso a AEM como registros de Cloud Service mediante la CLI [de E/S de](https://github.com/adobe/aio-cli) Adobe con el complemento [Cloud Manager para la CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager)de E/S de Adobe.

Primero, [configure la E/S de Adobe con el complemento](../../local-development-environment/development-tools.md#aio-cli)Cloud Manager.

Asegúrese de que se han identificado el ID de Programa y el ID de Entorno relevantes, y utilice las opciones [de registro disponibles para la](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) lista para lista de las opciones de registro que se utilizan para [reducir](#aio-cli-tail-logs) o [descargar](#aio-cli-download-logs) los registros.

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

La CLI de E/S de Adobe permite rastrear los registros en tiempo real desde AEM como Cloud Service mediante el comando [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) . La segmentación es útil para ver la actividad de registros en tiempo real a medida que se realizan acciones en el AEM como entorno de Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Otras herramientas de la línea de comandos, como `grep` se pueden utilizar de manera conjunta con `tail-logs` para ayudar a aislar las declaraciones de registro de interés, por ejemplo:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... solo muestra las sentencias de registro generadas a partir de `com.example.MySlingModel` o que contienen esa cadena.

### Descarga de registros{#aio-cli-download-logs}

La CLI de E/S de Adobe permite descargar registros de AEM como Cloud Service mediante el comando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Esto proporciona el mismo resultado final que descargar los registros de la interfaz de usuario web del Administrador de nubes, con la diferencia de que el `download-logs` comando consolida los registros a lo largo de los días, en función de la cantidad de días de registro solicitados.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Explicación de los registros

Los inicios de sesión AEM como Cloud Service tienen varios pods escribiendo sentencias de registro en ellos. Dado que varias instancias de AEM escriben en el mismo archivo de registro, es importante comprender cómo analizar y reducir el ruido durante la depuración. Para explicarlo, se utilizará el siguiente fragmento `aemerror` de registro:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Con los ID de pod, el punto de datos después de la fecha y hora, los registros se pueden recopilar mediante el pod o AEM instancia dentro del servicio, lo que facilita el seguimiento y la comprensión de la ejecución del código.

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

Las directrices generales del Adobe sobre los niveles de registro por AEM como entorno del Cloud Service son:

+ Desarrollo local (AEM SDK): `DEBUG`
+ Desarrollo: `DEBUG`
+ Escenario: `WARN`
+ Producción: `ERROR`

La configuración del nivel de registro más adecuado para cada tipo de entorno se realiza con AEM como Cloud Service; los niveles de registro se mantienen en el código

+ Las configuraciones de registro de Java se mantienen en configuraciones de OSGi
+ Los niveles de registro del servidor web Apache y Dispatcher en el proyecto de distribuidor

...y, por lo tanto, requieren un despliegue para cambiar.

### Entorno de variables específicas para establecer los niveles de registro de Java

Una alternativa a establecer niveles de registro Java estáticos y conocidos para cada entorno es utilizar AEM como variables [específicas de](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) entorno de Cloud Service para parametrizar los niveles de registro, permitiendo que los valores se cambien dinámicamente mediante la CLI de E/S de [Adobe con el complemento](#aio-cli)Cloud Manager.

Esto requiere actualizar las configuraciones OSGi de registro para utilizar los marcadores de posición de variables específicos de entorno. [Los valores](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) predeterminados para los niveles de registro deben establecerse según las recomendaciones [de](#log-levels)Adobe. Por ejemplo:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config-example.cfg.json`

```
{
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
}
```

Este enfoque presenta aspectos negativos que deben tenerse en cuenta:

+ [Se permite](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)un número limitado de variables de entorno y la creación de una variable para administrar el nivel de registro utilizará una.
+ Las variables de entorno solo se pueden administrar mediante programación mediante la CLI [de E/S de](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) Adobe o las API [HTTP de](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties)Cloud Manager.
+ Los cambios en las variables de entorno deben restablecerse manualmente mediante una herramienta compatible. Olvidar restablecer un entorno de tráfico alto, como Producción, a un nivel de registro menos detallado puede inundar los registros e influir en el rendimiento AEM.

_Las variables específicas de entorno no funcionan para las configuraciones del registro de Dispatcher o del servidor web Apache, ya que no se configuran mediante la configuración OSGi._