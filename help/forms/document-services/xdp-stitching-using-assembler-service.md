---
title: Vinculación XDP mediante el servicio de ensamblador
description: Usar el servicio Assembler en AEM Forms para unir xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
duration: 91
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Configuración XDP mediante el servicio de ensamblador

Este artículo proporciona los recursos para mostrar la capacidad de unir documentos xdp mediante el servicio Assembler.
El siguiente código jsp se escribió para insertar un subformulario llamado **dirección** desde un documento xdp llamado address.xdp hasta un punto de inserción llamado **dirección** en el documento master.xdp. AEM El xdp resultante se guardó en la carpeta raíz de la instalación de la.

El servicio Assembler se basa en documentos DDX válidos para describir la manipulación de documentos de PDF. Puede consultar el [Documento de referencia DDX aquí](assets/ddxRef.pdf).La página 40 contiene información sobre la vinculación xdp.

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

A continuación, se muestra el archivo DDX para insertar fragmentos en otro xdp. El DDX inserta el subformulario  **dirección** desde address.xdp hasta el punto de inserción llamado **dirección** en master.xdp. El documento resultante denominado **stitched.xdp** se guarda en el sistema de archivos.

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

AEM Para que esta capacidad funcione en el servidor de la

* Descargar [Paquete de vinculación XDP](assets/xdp-stitching.zip) a su sistema local.
* Cargue e instale el paquete utilizando [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* [Extraiga el contenido de este archivo zip](assets/xdp-and-ddx.zip) para obtener el archivo xdp y DDX de muestra

**Después de instalar el paquete tendrá que lista de permitidos las siguientes URL en Adobe Granite CSRF Filter.**

1. Siga los pasos que se mencionan a continuación para lista de permitidos las rutas mencionadas anteriormente.
1. [Inicie sesión en configMgr](http://localhost:4502/system/console/configMgr)
1. Búsqueda de Adobe Granite CSRF Filter
1. Añada la siguiente ruta en las secciones excluidas y guarde `/content/AemFormsSamples/assemblerservice`
1. Busque &quot;Filtro de referente de Sling&quot;
1. Marque la casilla &quot;Permitir vacío&quot;. (Esta configuración solo debe ser para fines de prueba) Existen varias formas de probar el código de muestra. La forma más rápida y sencilla de usar una aplicación de Postman es. Postman le permite realizar solicitudes de POST al servidor. Instale la aplicación de Postman en su sistema.
Inicie la aplicación e introduzca la siguiente URL para probar la API de exportación de datos http://localhost:4502/content/AemFormsSamples/assemblerservice.html

Proporcione los siguientes parámetros de entrada según se especifican en la captura de pantalla. Puede utilizar los documentos de ejemplo que descargó anteriormente,
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Asegúrese de que la instalación de AEM Forms haya finalizado. Todos los paquetes deben estar en estado activo.
>
