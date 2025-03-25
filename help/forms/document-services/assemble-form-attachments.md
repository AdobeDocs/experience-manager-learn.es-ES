---
title: Montar archivos adjuntos del formulario
description: Montar los archivos adjuntos del formulario en el orden especificado
feature: Assembler
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6406
thumbnail: kt-6406.jpg
topic: Development
role: Developer
level: Experienced
exl-id: a5df8780-b7ab-4b91-86f6-a24392752107
last-substantial-update: 2021-07-07T00:00:00Z
duration: 150
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 0%

---

# Montar archivos adjuntos del formulario

Este artículo proporciona recursos para ensamblar archivos adjuntos de formularios adaptables en un orden especificado. Los archivos adjuntos del formulario deben estar en formato pdf para que funcione este código de ejemplo. El siguiente es el caso de uso.
El usuario que rellena un formulario adaptable adjunta uno o más documentos pdf al formulario.
Al enviar el formulario, combine los archivos adjuntos del formulario para generar un PDF. Puede especificar el orden en que se ensamblan los archivos adjuntos para generar el PDF final.

## Crear un componente OSGi que implemente la interfaz WorkflowProcess

Cree un componente OSGi que implemente la interfaz [com.adobe.granite.workflow.exec.WorkflowProcess](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html). El código de este componente se puede asociar al componente paso de proceso en el flujo de trabajo de AEM. El método execute de la interfaz com.adobe.granite.workflow.exec.WorkflowProcess se implementa en este componente.

Cuando se envía un formulario adaptable al déclencheur de un flujo de trabajo de AEM, los datos enviados se almacenan en el archivo especificado, en la carpeta de carga útil. Por ejemplo, este es el archivo de datos enviado. Necesitamos montar los documentos adjuntos especificados bajo la etiqueta de identificación y estados bancarios.
![datos enviados](assets/submitted-data.JPG).

### Obtener los nombres de las etiquetas

El orden de los archivos adjuntos se especifica como argumentos de paso de proceso en el flujo de trabajo, como se muestra en la captura de pantalla siguiente. Aquí estamos reuniendo los anexos añadidos al campo de identificación seguidos de los estados de cuenta

![paso-proceso](assets/process-step.JPG)

El siguiente fragmento de código extrae los nombres de datos adjuntos de los argumentos de proceso

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Crear DDX a partir de los nombres de archivos adjuntos

Luego necesitamos crear el documento [Document Description XML (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) que usa el servicio Assembler para ensamblar documentos. El siguiente es el DDX que se creó a partir de los argumentos de proceso. El elemento NoForms permite aplanar documentos basados en XFA antes de montarlos. Observe que los elementos de origen de PDF están en el orden correcto, tal como se especifica en los argumentos de proceso.

![ddx-xml](assets/ddx.PNG)

### Creación de un mapa de documentos

A continuación, creamos un mapa de documentos con el nombre del archivo adjunto como clave y el archivo adjunto como valor. El servicio Query Builder se utilizaba para consultar los archivos adjuntos en la ruta de carga útil y crear el mapa de documentos. Este mapa del documento junto con el DDX es necesario para que el servicio de ensamblador combine el PDF final.

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

### Utilizar AssemblerService para ensamblar los documentos

Una vez creados el DDX y el mapa del documento, el siguiente paso es utilizar AssemblerService para montar los documentos.
El siguiente código monta y devuelve el PDF ensamblado.

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

### Guarde el PDF ensamblado en la carpeta de carga útil

El paso final es guardar el PDF ensamblado en la carpeta de carga útil. A continuación, se puede acceder a este pdf en los pasos siguientes del flujo de trabajo para un procesamiento posterior.
El siguiente fragmento de código se utilizó para guardar el archivo en la carpeta de carga útil

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

A continuación se muestra la estructura de carpetas de carga útil después de ensamblar y almacenar los archivos adjuntos del formulario.

![estructura de carga útil](assets/payload-structure.JPG)

### Para que esta capacidad funcione en el servidor de AEM

* Descargue [Montar formulario de archivos adjuntos](assets/assemble-form-attachments-af.zip) en su sistema local.
* Importe el formulario desde la página [Forms And Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Descargue [workflow](assets/assemble-form-attachments.zip) e impórtelo a AEM mediante el administrador de paquetes.
* Descargar el [paquete personalizado](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* Implementar e iniciar el paquete usando la [consola web](http://localhost:4502/system/console/bundles)
* Dirija su explorador a [AssembleAttachments Form](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* Agregue un archivo adjunto en el documento de ID y un par de documentos PDF a la sección de extractos bancarios
* Envíe el formulario al déclencheur del flujo de trabajo
* Compruebe la carpeta [payload del flujo de trabajo en el crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) para el pdf ensamblado

>[!NOTE]
> Si ha habilitado el registrador para el paquete personalizado, el DDX y el archivo ensamblado se escribirán en la carpeta de la instalación de AEM.
