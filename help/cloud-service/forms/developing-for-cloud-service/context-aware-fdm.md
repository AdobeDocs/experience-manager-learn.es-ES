---
title: Compatibilidad con la anulación de la configuración según el contexto para el modelo de datos de formulario
description: Configure los modelos de datos de formulario para comunicarse con diferentes puntos finales según los entornos.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 0%

---

# Configuraciones de nube según el contexto

Al crear la configuración de nube en su entorno local y en pruebas exitosas, querrá usar la misma configuración de nube en los entornos de flujo ascendente, pero sin tener que cambiar el extremo, la clave/contraseña secreta y el nombre de usuario. Para lograr este caso de uso, AEM Forms en Cloud Service ha introducido la capacidad de definir configuraciones de nube según el contexto.
Por ejemplo, la configuración de la nube de cuentas de almacenamiento de Azure se puede reutilizar en entornos de desarrollo, fase y producción utilizando diferentes cadenas de conexión y claves para .

Se necesitan los siguientes pasos para crear la configuración de nube según el contexto

## Crear variables de entorno

Las variables de entorno estándar se pueden configurar y administrar mediante Cloud Manager. Se proporcionan al entorno de tiempo de ejecución y se pueden utilizar en configuraciones OSGi. [Las variables de entorno pueden ser valores específicos de entorno o secretos de entorno, según lo que se esté cambiando.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



La siguiente captura de pantalla muestra las variables de entorno azure_key y azure_connection_string definidas
![environment_variables](assets/environment-variables.png)

Estas variables de entorno se pueden especificar en los archivos de configuración para utilizarlos en el entorno adecuado. Por ejemplo, si desea que todas las instancias de autor utilicen estas variables de entorno, definirá el archivo de configuración en la carpeta config.author como se especifica a continuación

## Crear archivo de configuración

Abra el proyecto en IntelliJ. Vaya a config.author y cree un archivo llamado

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

Copie el siguiente texto en el archivo que creó en el paso anterior. El código de este archivo anula el valor de las propiedades accountName y accountKey con las variables de entorno **azure_connection_string** y **azure_key**.

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>Esta configuración se aplicará a todos los entornos de creación de la instancia de servicio en la nube. Para aplicar la configuración a los entornos de publicación, deberá colocar el mismo archivo de configuración en la carpeta config.publish del proyecto intelliJ
>[!NOTE]
> Asegúrese de que la propiedad que se está anulando sea una propiedad válida de la configuración de nube. Vaya a la configuración de la nube para encontrar la propiedad que desea anular, como se muestra a continuación.

![cloud-config-property](assets/cloud-config-properties.png)

Para la configuración de nube basada en REST con autenticación básica, normalmente desea crear variables de entorno para las propiedades serviceEndPoint, userName y password.
