---
title: Compilación de archivos adjuntos de formulario
description: Compilación de datos adjuntos de formulario en el orden especificado
feature: assembler
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 6406
thumbnail: kt-6406.jpg
translation-type: tm+mt
source-git-commit: 3e8b820939c2d39ef9a17f7d7aaef87cd9cdbbbb
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 0%

---


# Compilación de archivos adjuntos de formulario

En este artículo se proporcionan recursos para ensamblar datos adjuntos de formularios adaptables en un orden especificado. Los archivos adjuntos del formulario deben estar en formato pdf para que este código de muestra funcione. El siguiente es el caso de uso.
El usuario que rellena un formulario adaptable adjunta uno o varios documentos PDF al formulario.
Al enviar el formulario, cree los anexos del formulario para generar un PDF. Puede especificar el orden en que se ensamblan los archivos adjuntos para generar el PDF final.

## Crear un componente OSGi que implemente la interfaz WorkflowProcess

Cree un componente OSGi que implemente la [com.adobe.granite.workflow.exec.WorkflowProcess interfaz](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/exec/WorkflowProcess.html). El código de este componente puede asociarse con el componente de paso de proceso del flujo de trabajo de AEM. El método execute de la interfaz com.adobe.granite.workflow.exec.WorkflowProcess se implementa en este componente.

Cuando se envía un formulario adaptable a déclencheur y un flujo de trabajo AEM, los datos enviados se almacenan en el archivo especificado en la carpeta de carga útil. Por ejemplo, este es el archivo de datos enviado. Tenemos que reunir los datos adjuntos especificados en la etiqueta de identificación y de declaración de billetes.
![submit-data](assets/submitted-data.JPG).

### Obtener los nombres de las etiquetas

El orden de los archivos adjuntos se especifica como argumentos de paso de proceso en el flujo de trabajo, como se muestra en la captura de pantalla siguiente. Aquí estamos ensamblando los anexos añadidos al código de identificación de campo seguido de los estados financieros

![proceso-paso](assets/process-step.JPG)

El siguiente fragmento de código extrae los nombres de los datos adjuntos de los argumentos de proceso

```java
String  []attachmentNames  = arg2.get("PROCESS_ARGS","string").toString().split(",");
```

### Crear DDX a partir de los nombres de los archivos adjuntos

A continuación, necesitamos crear el documento [XML de descripción de Documento (DDX)](https://helpx.adobe.com/pdf/aem-forms/6-2/ddxRef.pdf) que utiliza el servicio Ensamblador para ensamblar documentos. El siguiente es el DDX que se creó a partir de los argumentos de proceso. El elemento NoForms permite acoplar documentos basados en XFA antes de montarlos. Observe que los elementos de origen PDF están en el orden correcto, tal como se especifica en los argumentos del proceso.

![ddx-xml](assets/ddx.PNG)

### Crear mapa de documentos

A continuación, creamos un mapa de documentos con el nombre del archivo adjunto como clave y el archivo adjunto como valor. El servicio del generador de consultas se utilizó para consulta de los datos adjuntos en la ruta de carga útil y crear el mapa de documentos. Este mapa de documento junto con el DDX es necesario para que el servicio de montaje pueda montar el PDF final.

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

### Utilice AssemblyService para montar los documentos

Después de crear el DDX y el mapa de documento, el paso siguiente es utilizar el servicio de ensamblador para montar los documentos.
El siguiente código ensambla y devuelve el PDF ensamblado.

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

El paso final es guardar el PDF ensamblado en la carpeta de carga útil. A continuación, se puede acceder a este archivo PDF en los pasos subsiguientes del flujo de trabajo para un procesamiento posterior.
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

A continuación se muestra la estructura de la carpeta de carga útil después de ensamblar y almacenar los datos adjuntos del formulario.

![payload-structure](assets/payload-structure.JPG)

### Para que esta capacidad funcione en el servidor AEM

* Descargue el [Formulario para ensamblar archivos adjuntos de formulario](assets/assemble-form-attachments-af.zip) en su sistema local.
* Importe el formulario desde la página[Forms y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Descargue [workflow](assets/assemble-form-attachments.zip) e importe en AEM mediante el administrador de paquetes.
* Descargue el [paquete personalizado](assets/assembletaskattachments.assembletaskattachments.core-1.0-SNAPSHOT.jar)
* Implementar y inicio del paquete mediante la [consola web](http://localhost:4502/system/console/bundles)
* Apunta a tu explorador para [EnsamblarFormulario de datos adjuntos](http://localhost:4502/content/dam/formsanddocuments/assembleattachments/jcr:content?wcmmode=disabled)
* Añadir un archivo adjunto en el Documento de ID y un par de documentos en pdf en la sección de extractos bancarios
* Enviar el formulario para déclencheur del flujo de trabajo
* Consulte la carpeta [payload del flujo de trabajo en crx](http://localhost:4502/crx/de/index.jsp#/var/fd/dashboard/payload) para ver el PDF ensamblado

>[!NOTE]
> Si ha habilitado el registrador para el paquete personalizado, el DDX y el archivo ensamblado se escriben en la carpeta de la instalación de AEM.

