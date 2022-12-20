---
title: Vinculación XDP mediante el servicio de ensamblador
description: Uso del servicio Assembler en AEM Forms para unir xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
source-git-commit: 8f17e98c56c78824e8850402e3b79b3d47901c0b
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 2%

---

# Vinculación XDP con servicio de ensamblador

Este artículo le proporciona los recursos para demostrar la capacidad de unir documentos xdp mediante el servicio de ensamblador.
Se escribió el siguiente código jsp para insertar un subformulario llamado **address** desde el documento xdp llamado address.xdp en un punto de inserción llamado **address** en el documento master.xdp. El xdp resultante se guardó en la carpeta raíz de la instalación de AEM.

El servicio del ensamblador se basa en documentos DDX válidos para describir la manipulación de documentos PDF. Puede consultar la [Documento de referencia DDX aquí](assets/ddxRef.pdf).La página 40 tiene información sobre la vinculación de xdp.

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

El archivo DDX para insertar fragmentos en otro xdp se muestra a continuación. El DDX inserta el subformulario  **address** de address.xdp al punto de inserción llamado **address** en el archivo master.xdp. El documento resultante llamado **stitched.xdp** se guarda en el sistema de archivos.

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

Para que esta capacidad funcione en el servidor AEM

* Descargar [Paquete de conexión XDP](assets/xdp-stitching.zip) al sistema local.
* Cargue e instale el paquete mediante el [gestor de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Extraer el contenido de este archivo zip](assets/xdp-and-ddx.zip) para obtener el archivo xdp y DDX de muestra

**Después de instalar el paquete tendrá que lista de permitidos las siguientes URL en el Adobe Granite CSRF Filter.**

1. Siga los pasos que se indican a continuación para realizar la lista de permitidos de las rutas mencionadas anteriormente.
1. [Iniciar sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar el filtro de Adobe Granite CSRF
1. Añada la siguiente ruta en las secciones excluidas y guarde `/content/AemFormsSamples/assemblerservice`
1. Buscar &quot;Filtro de referente de Sling&quot;
1. Marque la casilla de verificación &quot;Permitir vacío&quot;. (Esta configuración solo debe utilizarse con fines de prueba) Existen varias formas de probar el código de muestra. Lo más rápido y sencillo es usar la aplicación Postman. Postman le permite realizar solicitudes de POST al servidor. Instale la aplicación de Postman en su sistema.
Inicie la aplicación e introduzca la siguiente URL para probar la API de datos de exportación http://localhost:4502/content/AemFormsSamples/assemblerservice.html

Proporcione los siguientes parámetros de entrada como se especifica en la captura de pantalla. Puede utilizar los documentos de ejemplo que descargó anteriormente,
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Asegúrese de que la instalación de AEM Forms se haya completado. Todos sus paquetes deben estar en estado activo.
