---
title: Montaje de archivos adjuntos de formulario
description: Montaje de archivos adjuntos de formulario en el orden especificado
feature: Assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---


# Montaje de archivos adjuntos de formulario

Este artículo proporciona recursos para ensamblar archivos adjuntos de formularios adaptables en un orden especificado. Los archivos adjuntos del formulario deben estar en formato pdf para que funcione este código de ejemplo. El siguiente es el caso de uso.
El usuario que rellena un formulario adaptable adjunta uno o varios documentos pdf al formulario.
Al enviar el formulario, ensamblar los archivos adjuntos del formulario para generar un pdf. Puede especificar el orden en que se ensamblan los archivos adjuntos para generar el pdf final.

## Crear un componente OSGi que implemente la interfaz WorkflowProcess

Cree un componente OSGi que implemente la [com.adobe.granite.workflow.exec.WorkflowProcess interface](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html). El código de este componente se puede asociar al componente de paso del proceso en el flujo de trabajo de AEM. El método execute de la interfaz com.adobe.granite.workflow.exec.WorkflowProcess se implementa en este componente.

Cuando se envía un formulario adaptable para activar un flujo de trabajo de AEM, los datos enviados se almacenan en el archivo especificado en la carpeta de carga útil. Por ejemplo, este es el archivo de datos enviado. Necesitamos ensamblar los adjuntos especificados en la etiqueta de identificación y de declaración de billetes.
![datos enviados](assets/submitted-data.JPG).

### Obtener los nombres de las etiquetas

El orden de los archivos adjuntos se especifica como argumentos de paso del proceso en el flujo de trabajo, como se muestra en la captura de pantalla siguiente. Aquí se ensamblan los anexos añadidos a la tarjeta de identificación de campo, seguidos de los estados de cuenta

![paso del proceso](assets/process-step.JPG)

El siguiente fragmento de código extrae los nombres de los archivos adjuntos de los argumentos de proceso

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Crear DDX a partir de los nombres de archivos adjuntos

A continuación, es necesario crear el documento [Document Description XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) que utiliza el servicio Assembler para ensamblar documentos. El siguiente es el DDX que se creó a partir de los argumentos de proceso. El elemento NoForms le permite acoplar documentos basados en XFA antes de montarlos. Observe que los elementos de origen PDF están en el orden correcto, tal como se especifica en los argumentos de proceso.

![ddx-xml](assets/ddx.PNG)

### Crear mapa de documentos

A continuación, creamos un mapa de documentos con el nombre del archivo adjunto como clave y el archivo adjunto como valor. El servicio Query Builder se utilizó para consultar los archivos adjuntos en la ruta de carga útil y crear el mapa de documentos. Este mapa del documento junto con el DDX es necesario para que el servicio ensamblador ensamble el pdf final.

```java
public Map<String, Object> createMapOfDocuments(String payloadPath,WorkflowSession workflowSession )
{
  Map<String, String> queryMap = new HashMap<String, String>();
  Map<String,Object>mapOfDocuments = new HashMap<String,Object>();
  queryMap.put("type", "nt:file");
  queryMap.put("path",payloadPath);
  Query query = queryBuilder.createQuery(PredicateGroup.create(queryMap),workflowSession.adaptTo(Session.class));
  query.setStart(0);
  query.setHitsPerPage(30);
  SearchResult result = query.getResult();
  log.debug("Get result hits "+result.getHits().size());
  for (Hit hit : result.getHits()) {
    try {
          String path = hit.getPath();
          log.debug("The title "+hit.getTitle()+" path "+path);
          if(hit.getTitle().endsWith("pdf"))
           {
             com.adobe.aemfd.docmanager.Document attachmentDocument = new com.adobe.aemfd.docmanager.Document(path);
             mapOfDocuments.put(hit.getTitle(),attachmentDocument);
             log.debug("@@@@Added to map@@@@@ "+hit.getTitle());
           }
        }
    catch (Exception e)
       {
          log.debug(e.getMessage());
       }

}
return mapOfDocuments;
}
```

### Utilice AssemblerService para ensamblar los documentos

Después de crear el DDX y el mapa de documentos, el siguiente paso es el uso de AssemblerService para ensamblar los documentos.
El siguiente código monta y devuelve el pdf montado.

```java
private com.adobe.aemfd.docmanager.Document assembleDocuments(Map<String, Object> mapOfDocuments, com.adobe.aemfd.docmanager.Document ddxDocument)
{
    AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
    aoSpec.setFailOnError(true);
    AssemblerResult ar = null;
    try
    {
        ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
        return (com.adobe.aemfd.docmanager.Document) ar.getDocuments().get("GeneratedDocument.pdf");
    }
    catch (OperationException e)
    {
        log.debug(e.getMessage());
    }
    return null;
    
}
```

### Guarde el pdf montado en la carpeta de carga útil

El paso final es guardar el pdf ensamblado en la carpeta de carga útil. A continuación, se puede acceder a este pdf en los pasos siguientes del flujo de trabajo para un procesamiento más detallado.
Se ha utilizado el siguiente fragmento de código para guardar el archivo en la carpeta de carga útil

```java
Session session = workflowSession.adaptTo(Session.class);
javax.jcr.Node payloadNode =  workflowSession.adaptTo(Session.class).getNode(workItem.getWorkflowData().getPayload().toString());
log.debug("The payload Path is "+payloadNode.getPath());
javax.jcr.Node assembledPDFNode = payloadNode.addNode("assembled-pdf.pdf", "nt:file"); 
javax.jcr.Node jcrContentNode =  assembledPDFNode.addNode("jcr:content", "nt:resource");
Binary binary =  session.getValueFactory().createBinary(assembledDocument.getInputStream());
jcrContentNode.setProperty("jcr:data", binary);
log.debug("Saved !!!!!!"); 
session.save();
```

A continuación se muestra la estructura de la carpeta de carga útil después de ensamblar y almacenar los archivos adjuntos.

![payload-structure](assets/payload-structure.JPG)

### Para que esta capacidad funcione en su servidor AEM

* Descargue el [Formulario de ensamblados de archivos adjuntos](assets/assemble-form-attachments-af.zip) en su sistema local.
* Importe el formulario desde la página [Forms and Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Descargue [workflow](assets/assemble-form-attachments.zip) e impórtelo en AEM mediante el administrador de paquetes.
* Descargue el [paquete personalizado](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* Implemente e inicie el paquete utilizando la [consola web](http://localhost:4502/system/console/bundles)
* Apunte el navegador a [Assemble Attachments Form](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* Agregue un adjunto en el documento de ID y un par de documentos pdf a la sección extractos bancarios
* Enviar el formulario para activar el flujo de trabajo
* Compruebe la carpeta [payload del flujo de trabajo en el crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) para el pdf ensamblado

>[!NOTE]
> Si ha habilitado el registrador para el paquete personalizado, el DDX y el archivo ensamblado se escriben en la carpeta de la instalación de AEM.

