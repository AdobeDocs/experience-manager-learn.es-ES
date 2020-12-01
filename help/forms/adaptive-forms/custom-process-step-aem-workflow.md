---
title: Implementación de paso de proceso personalizado
seo-title: Implementación de paso de proceso personalizado
description: Escritura de datos adjuntos de formularios adaptables en el sistema de archivos mediante el paso de proceso personalizado
seo-description: Escritura de datos adjuntos de formularios adaptables en el sistema de archivos mediante el paso de proceso personalizado
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '895'
ht-degree: 0%

---


# Paso de proceso personalizado

Este tutorial está dirigido a los clientes de AEM Forms que necesiten implementar el paso de proceso personalizado. Un paso del proceso puede ejecutar una secuencia de comandos ECMA o llamar al código Java personalizado para realizar operaciones. En este tutorial se explican los pasos necesarios para implementar WorkflowProcess que se ejecuta con el paso del proceso.

El principal motivo para implementar el paso de proceso personalizado es ampliar el flujo de trabajo AEM. Por ejemplo, si utiliza componentes de AEM Forms en el modelo de flujo de trabajo, puede que desee realizar las siguientes operaciones

* Guardar los datos adjuntos del formulario adaptable en el sistema de archivos
* Manipular los datos enviados

Para llevar a cabo el caso de uso anterior, normalmente escribirá un servicio OSGi que se ejecuta mediante el paso del proceso.

## Crear proyecto Maven

El primer paso consiste en crear un proyecto concreto utilizando el arquetipo Maven de Adobe apropiado. Los pasos detallados se enumeran en este [artículo](https://helpx.adobe.com/experience-manager/using/maven_arch13.html). Una vez que haya importado el proyecto simulado en eclipse, estará listo para escribir el inicio del primer componente OSGi que se pueda utilizar en el paso del proceso.


### Crear clase que implemente WorkflowProcess

Abra el proyecto móvil en el IDE de eclipse. Expanda la carpeta **nombreproyecto** > **núcleo**. Expanda la carpeta src/main/java. Debería ver un paquete que termina con &quot;core&quot;. Cree una clase Java que implemente WorkflowProcess en este paquete. Deberá anular el método execute. La firma del método execute es la siguiente
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)genera WorkflowException
El método execute proporciona acceso a las siguientes 3 variables

**WorkItem**: La variable workItem proporcionará acceso a los datos relacionados con el flujo de trabajo. La documentación de API pública está disponible [aquí.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Esta variable workflowSession le permitirá controlar el flujo de trabajo. La documentación de API pública está disponible [aquí](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**: Todos los metadatos asociados al flujo de trabajo. Todos los argumentos de proceso que se pasan al paso de proceso están disponibles mediante el objeto MetaDataMap.[Documentación de API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

En este tutorial, vamos a escribir los archivos adjuntos agregados al formulario adaptable en el sistema de archivos como parte del flujo de trabajo de AEM.

Para llevar a cabo este caso de uso, se escribió la siguiente clase java

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

Línea 1: define las propiedades de nuestro componente. La propiedad process.label es lo que verá al asociar el componente OSGi con el paso del proceso, como se muestra en una de las capturas de pantalla siguientes.

Líneas 13-15: los argumentos de proceso pasados a este componente OSGi se dividen mediante el separador &quot;,&quot;. Los valores de attachmentPath y saveToLocation se extraen de la matriz de cadenas.

* attachmentPath: es la misma ubicación que especificó en el formulario adaptable cuando configuró la acción de envío del formulario adaptable para invocar AEM flujo de trabajo. Es un nombre de la carpeta en la que desea guardar los archivos adjuntos en AEM con relación a la carga útil del flujo de trabajo.

* saveToLocation: es la ubicación en la que desea que los archivos adjuntos se guarden en el sistema de archivos del servidor de AEM.

Estos dos valores se pasan como argumentos de proceso, como se muestra en la siguiente captura de pantalla.

![ProcessStep](assets/implement-process-step.gif)


Línea 19: luego construimos el archivo attachmentFilePath. La ruta del archivo de datos adjuntos es similar a

    /var/fd/panel/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Attachments/attachments

* &quot;Datos adjuntos&quot; es el nombre de la carpeta relativa a la carga útil del flujo de trabajo que se especificó al configurar la opción de envío del formulario adaptable.

   ![opciones de envío](assets/af-submit-options.gif)

Líneas 24-26: Get ResourceResolver y, a continuación, el recurso que apunta a attachmentFilePath.

El resto del código crea objetos de Documento iterando a través del objeto secundario del recurso que apunta a attachmentFilePath mediante la API. Este objeto documento es específico de AEM Forms. A continuación, se utiliza el método copyToFile del objeto documento para guardar el objeto documento.

>[!NOTE]
>
>Dado que utilizamos un objeto de Documento específico de AEM Forms, es necesario incluir la dependencia aemfd-client-sdk en el proyecto principal. El identificador de grupo es com.adobe.aemfd y el identificador de artefacto es aemfd-client-sdk.

#### Generar e implementar

[Genere el paquete como se describe ](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[aquíAsegúrese de que el paquete está implementado y en estado activo](http://localhost:4502/system/console/bundles)

Cree un modelo de flujo de trabajo. Arrastre y suelte el paso del proceso en el modelo de flujo de trabajo. Asocie el paso del proceso con &quot;Guardar datos adjuntos de formulario adaptables al sistema de archivos&quot;.

Proporcione los argumentos de proceso necesarios separados por coma. Por ejemplo, Archivos adjuntos,c:\\scrappp\\. El primer argumento es la carpeta en la que se van a almacenar los archivos adjuntos del formulario adaptable en relación con la carga útil del flujo de trabajo. Debe ser el mismo valor que especificó al configurar la acción de envío de formulario adaptable. El segundo argumento es la ubicación en la que desea que se almacenen los archivos adjuntos.

Crear un formulario adaptable. Arrastre y suelte el componente Archivos adjuntos en el formulario. Configure la acción de envío del formulario para que invoque el flujo de trabajo creado en los pasos anteriores. Proporcione la ruta de datos adjuntos adecuada.

Guarde la configuración.

Obtener una vista previa del formulario. Añada un par de datos adjuntos y envíe el formulario. Los archivos adjuntos deben guardarse en el sistema de archivos en la ubicación especificada por usted en el flujo de trabajo.

