---
title: Compatibilidad de anulación de configuración según el contexto con el modelo de datos de formulario
description: Configure los modelos de datos de formulario para comunicarse con diferentes puntos finales según los entornos.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
duration: 80
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 7%

---

# Configuraciones de nube según el contexto

Al crear la configuración de nube en el entorno local y al realizar pruebas correctamente, le interesa utilizar la misma configuración de nube en los entornos de flujo ascendente, pero sin tener que cambiar el punto de conexión, la clave/contraseña secreta y/o el nombre de usuario. Para aplicar este caso de uso, AEM Forms en Cloud Service ha introducido la capacidad de definir configuraciones de nube según el contexto.
Por ejemplo, la configuración de nube de la cuenta de almacenamiento de Azure se puede reutilizar en entornos de desarrollo, fase y producción mediante diferentes cadenas de conexión y claves para.

Se necesitan los siguientes pasos para crear una configuración de nube según el contexto

## Crear variables de entorno

Las variables de entorno estándar se pueden configurar y administrar mediante Cloud Manager. Se proporcionan al entorno del tiempo de ejecución y se pueden utilizar en configuraciones OSGi. [Las variables de entorno pueden ser valores específicos del entorno o secretos del entorno, según lo que se vaya a cambiar.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=en)



La captura de pantalla siguiente muestra las variables de entorno azure_key y azure_connection_string definidas
![variables_de_entorno](assets/environment-variables.png)

Estas variables de entorno se pueden especificar en los archivos de configuración para utilizarlas en el entorno adecuado
Por ejemplo, si desea que todas las instancias de autor utilicen estas variables de entorno, defina el archivo de configuración en la carpeta config.author como se especifica a continuación

## Crear archivo de configuración

Abra el proyecto en IntelliJ. Vaya a config.author y cree un archivo llamado

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png)

Copie el siguiente texto en el archivo que creó en el paso anterior. El código de este archivo está anulando el valor de las propiedades accountName y accountKey con las variables de entorno **azure_connection_string** y **azure_key**.

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
>Esta configuración se aplicará a todos los entornos de creación de la instancia de Cloud Service. Para aplicar la configuración a entornos de publicación tendrá que colocar el mismo archivo de configuración en la carpeta config.publish del proyecto intelliJ
>[!NOTE]
> Asegúrese de que la propiedad que se está anulando sea una propiedad válida de la configuración de la nube. Vaya a la configuración de nube para encontrar la propiedad que desea anular, como se muestra a continuación.

![cloud-config-property](assets/cloud-config-properties.png)

En el caso de las configuraciones de nube basadas en REST con autenticación básica, normalmente deseará crear variables de entorno para las propiedades serviceEndPoint, userName y password.

## Siguientes pasos

[AEM Insertar el proyecto de la en Cloud Manager](./push-project-to-cloud-manager-git.md)
