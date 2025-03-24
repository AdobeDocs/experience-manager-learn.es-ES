---
title: Corrija los errores de tiempo de espera al combinar archivos de datos xml grandes con la plantilla xdp
description: Combinación de archivos xml grandes con plantillas en AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: Experience Manager 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 37
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 1%

---

# Cómo habilitar la creación de archivos pdf combinando archivos de datos xml grandes con plantillas xdp

Al combinar archivos de datos xml grandes con la plantilla xdp, puede ver el siguiente error en el archivo de registro

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Para corregir el error anterior, haga lo siguiente

## Cambio del tiempo de espera de aries

* Detener AEM Server
* Cree una carpeta llamada **install** en la carpeta crx-quickstart de la instalación de AEM
* Cree un archivo llamado **org.apache.aries.transaction.config** con el siguiente contenido
aries.transaction.timeout=&quot;1200&quot;
en la carpeta de instalación. Puede cambiar el valor de tiempo de espera según sus necesidades. El valor de tiempo de espera es en segundos

>[!NOTE]
> Una vez que haya creado la configuración org.apache.aries.transaction, podrá editar los valores de tiempo de espera de la transacción desde [configMgr](http://localhost:4502/system/console/configMgr) en lugar de editar el archivo


## Cambiar la configuración del proveedor Jacorb ORB

* [Abrir el Configuration Manager de OSGi](http://localhost:4502/system/console/configMgr)
* Busque **Jacorb ORB Provider**
* Añadir la siguiente entrada
jacorb.connection.client.pending_reply_timeout=600000
La configuración anterior establece el tiempo de espera de respuesta pendiente (también conocido como tiempo de espera del cliente CORBA) en 600 segundos.
* Guarde los cambios
