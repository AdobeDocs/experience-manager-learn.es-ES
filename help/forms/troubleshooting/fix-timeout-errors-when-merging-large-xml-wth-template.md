---
title: Corrija los errores de tiempo de espera al combinar archivos de datos xml grandes con la plantilla xdp
description: Combinación de archivos xml grandes con plantillas en AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# Cómo habilitar la creación de archivos pdf combinando archivos de datos xml grandes con plantillas xdp

Al combinar archivos de datos xml grandes con la plantilla xdp, puede ver el siguiente error en el archivo de registro

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Para corregir el error anterior, haga lo siguiente

## Cambio del tiempo de espera de aries

* AEM Detener servidor
* Cree una carpeta llamada **instalar** AEM en la carpeta crx-quickstart de la instalación de la
* Cree un archivo llamado **org.apache.aries.transaction.config** con el siguiente contenido aries.transaction.timeout=&quot;1200&quot; en la carpeta de instalación. Puede cambiar el valor de tiempo de espera según sus necesidades. El valor de tiempo de espera es en segundos

>[!NOTE]
> Una vez creada la configuración org.apache.aries.transaction, puede editar los valores de tiempo de espera de transacción desde el [configMgr](http://localhost:4502/system/console/configMgr) en lugar de editar el archivo


## Cambiar la configuración del proveedor Jacorb ORB

* [Abra el Configuration Manager de OSGi](http://localhost:4502/system/console/configMgr)
* Buscar por **Jacorb ORB Provider**
* Agregue la siguiente entrada jacorb.connection.client.pending_reply_timeout=600000 La configuración anterior establece el tiempo de espera de respuesta pendiente (también conocido como tiempo de espera del cliente CORBA) en 600 segundos.
* Guarde los cambios
