---
title: Implementación del paso de proceso personalizado
seo-title: Implementación del paso de proceso personalizado
description: Escritura de archivos adjuntos de formularios adaptables en el sistema de archivos mediante el paso de proceso personalizado
seo-description: Escritura de archivos adjuntos de formularios adaptables en el sistema de archivos mediante el paso de proceso personalizado
feature: Flujo de trabajo
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '899'
ht-degree: 0%

---


# Paso de proceso personalizado

Este tutorial está diseñado para los clientes de AEM Forms que necesitan implementar el paso de proceso personalizado. Un paso del proceso puede ejecutar un script ECMA o llamar al código java personalizado para realizar operaciones. Este tutorial explica los pasos necesarios para implementar WorkflowProcess que se ejecuta en el paso de proceso.

La razón principal para implementar el paso de proceso personalizado es ampliar el flujo de trabajo de AEM. Por ejemplo, si utiliza componentes de AEM Forms en el modelo de flujo de trabajo, puede que desee realizar las siguientes operaciones

* Guarde los archivos adjuntos del formulario adaptable en el sistema de archivos
* Manipulación de los datos enviados

Para lograr el caso de uso anterior, normalmente escribirá un servicio OSGi que se ejecuta mediante el paso del proceso.

## Crear proyecto de Maven

El primer paso es crear un proyecto maven utilizando el tipo de archivo correspondiente de Adobe Maven. Los pasos detallados se enumeran en este [artículo](https://helpx.adobe.com/experience-manager/using/maven_arch13.html). Una vez que haya importado el proyecto maven en eclipse, estará listo para empezar a escribir su primer componente OSGi que se pueda utilizar en el paso del proceso.


### Crear clase que implemente WorkflowProcess

Abra el proyecto maven en su IDE de eclipse. Expanda la carpeta **projectname** > **core**. Expanda la carpeta src/main/java. Debería ver un paquete que termina con &quot;core&quot;. Cree una clase Java que implemente WorkflowProcess en este paquete. Deberá anular el método execute . La firma del método execute es la siguiente
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)lanza WorkflowException
El método execute permite acceder a las 3 variables siguientes

**WorkItem**: La variable workItem proporcionará acceso a los datos relacionados con el flujo de trabajo. La documentación de la API pública está disponible [aquí.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Esta variable workflowSession le permite controlar el flujo de trabajo. La documentación de la API pública está disponible [aquí](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**: Todos los metadatos asociados al flujo de trabajo. Los argumentos de proceso que se pasen al paso de proceso estarán disponibles mediante el objeto MetaDataMap .[Documentación de API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

En este tutorial, se escriben los archivos adjuntos añadidos al formulario adaptable al sistema de archivos como parte del flujo de trabajo de AEM.

Para lograr este caso de uso, se escribió la siguiente clase java

Veamos este código

```
@Component(property = { Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Save Adaptive Form Attachments to File System" })
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {
     private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
     @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
            throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
 
        String attachmentsFilePath = payloadPath + "/" + attachmentsPath + "/attachments";
        log.debug("The data file path is " + attachmentsFilePath);
 
        ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
 
        Resource attachmentsNode = resourceResolver.getResource(attachmentsFilePath);
        Iterator<Resource> attachments = attachmentsNode.listChildren();
        while (attachments.hasNext()) {
            Resource attachment = attachments.next();
            String attachmentPath = attachment.getPath();
            String attachmentName = attachment.getName();
 
            log.debug("The attachmentPath is " + attachmentPath + " and the attachmentname is " + attachmentName);
            com.adobe.aemfd.docmanager.Document attachmentDoc = new com.adobe.aemfd.docmanager.Document(attachmentPath,
                    attachment.getResourceResolver());
            try {
                File file = new File(saveToLocation + File.separator + workItem.getId());
                if (!file.exists()) {
                    file.mkdirs();
                }
 
                attachmentDoc.copyToFile(new File(file + File.separator + attachmentName));
 
                log.debug("Saved attachment" + attachmentName);
                attachmentDoc.close();
 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
 
        }
 
    }
```

Línea 1: define las propiedades de nuestro componente. La propiedad process.label es lo que verá al asociar el componente OSGi con el paso de proceso, como se muestra en una de las capturas de pantalla siguientes.

Líneas 13-15 - Los argumentos de proceso pasados a este componente OSGi se dividen utilizando el separador &quot;,&quot;. Los valores de attachmentPath y saveToLocation se extraen de la matriz de cadenas.

* attachmentPath : es la misma ubicación que especificó en el formulario adaptable cuando configuró la acción de envío del formulario adaptable para invocar el flujo de trabajo de AEM. Este es un nombre de la carpeta en la que desea que se guarden los archivos adjuntos en AEM en relación con la carga útil del flujo de trabajo.

* saveToLocation : es la ubicación en la que desea guardar los archivos adjuntos en el sistema de archivos del servidor AEM.

Estos dos valores se pasan como argumentos de proceso como se muestra en la captura de pantalla siguiente.

![ProcessStep](assets/implement-process-step.gif)


Línea 19: luego construimos el attachmentFilePath. La ruta del archivo adjunto es similar a

    /var/fd/dashboard/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Attachments/Attachments

* &quot;Attachments&quot; es el nombre de la carpeta relativa a la carga útil del flujo de trabajo que se especificó cuando configuró la opción de envío del formulario adaptable.

   ![opciones de envío](assets/af-submit-options.gif)

Líneas 24-26 - Obtenga ResourceResolver y, a continuación, el recurso que señala a attachmentFilePath.

El resto del código crea objetos Document al iterar el objeto secundario del recurso que señala attachmentFilePath mediante la API. Este objeto de documento es específico de AEM Forms. A continuación, se utiliza el método copyToFile del objeto de documento para guardar el objeto de documento.

>[!NOTE]
>
>Como se usa un objeto Document específico de AEM Forms, es necesario incluir la dependencia aemfd-client-sdk en el proyecto maven. El ID de grupo es com.adobe.aemfd y el id de artefacto es aemfd-client-sdk.

#### Generar e implementar

[Genere el paquete como se describe ](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[aquíAsegúrese de que el paquete esté implementado y en estado activo](http://localhost:4502/system/console/bundles)

Cree un modelo de flujo de trabajo. Arrastre y suelte el paso del proceso en el modelo de flujo de trabajo. Asocie el paso del proceso con &quot;Guardar archivos adjuntos de formulario adaptables en el sistema de archivos&quot;.

Proporcione los argumentos de proceso necesarios separados por una coma. Por ejemplo, archivos adjuntos, c:\\scrappp\\. El primer argumento es la carpeta en la que se van a almacenar los archivos adjuntos del formulario adaptable en relación con la carga útil del flujo de trabajo. Debe ser el mismo valor que especificó al configurar la acción de envío del formulario adaptable. El segundo argumento es la ubicación en la que desea que se almacenen los archivos adjuntos.

Crear un formulario adaptable. Arrastre y suelte el componente Archivos adjuntos en el formulario. Configure la acción de envío del formulario para invocar el flujo de trabajo creado en los pasos anteriores. Proporcione la ruta de conexión adecuada.

Guarde la configuración.

Obtener una vista previa del formulario. Añada un par de archivos adjuntos y envíe el formulario. Los archivos adjuntos deben guardarse en el sistema de archivos en la ubicación especificada por usted en el flujo de trabajo.

