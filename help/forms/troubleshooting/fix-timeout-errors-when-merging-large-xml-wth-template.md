---
title: Corrección de errores de tiempo de espera al combinar archivos de datos xml grandes con plantillas xdp
description: Combinación de archivos xml grandes con una plantilla en AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# Habilitación de la creación de archivos pdf combinando archivos de datos xml grandes con plantillas xdp

Al combinar archivos de datos xml de gran tamaño con una plantilla xdp, es posible que vea el siguiente error en el archivo de registro

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Para solucionar el error anterior, haga lo siguiente

## Cambiar el tiempo de espera de aries

* Detener AEM servidor
* Cree una carpeta llamada **instalar** en la carpeta crx-quickstart de la instalación de AEM
* Cree un archivo llamado **org.apache.aries.transaction.config** con el siguiente contenido aries.transaction.timeout=&quot;1200&quot; en la carpeta de instalación. Puede cambiar el valor de tiempo de espera según sus necesidades. El valor de tiempo de espera se expresa en segundos

>[!NOTE]
> Una vez que haya creado la configuración org.apache.aries.transaction , puede editar los valores de tiempo de espera de la transacción desde el [configMgr](http://localhost:4502/system/console/configMgr) en lugar de editar el archivo


## Cambiar la configuración del proveedor de Jacorb ORB

* [Abra OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* Buscar **Proveedor de Jacorb ORB**
* Agregue la siguiente entrada jacorb.connection.client.pending_reply_timeout=600000 La configuración anterior establece el tiempo de espera de respuesta pendiente (también conocido como tiempo de espera de cliente CORBA) en 600 segundos.
* Guarde los cambios
