---
title: Implementación del paso de proceso personalizado
description: Escritura de archivos adjuntos de formularios adaptables en el sistema de archivos mediante el paso de proceso personalizado
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 879518db-3f05-4447-86e8-5802537584e5
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '813'
ht-degree: 0%

---

# Paso de proceso personalizado

Este tutorial está diseñado para los clientes de AEM Forms que necesitan implementar el paso de proceso personalizado. Un paso del proceso puede ejecutar un script ECMA o llamar al código java personalizado para realizar operaciones. Este tutorial explica los pasos necesarios para implementar WorkflowProcess que se ejecuta en el paso de proceso.

La razón principal para implementar el paso de proceso personalizado es ampliar el flujo de trabajo AEM. Por ejemplo, si utiliza componentes de AEM Forms en el modelo de flujo de trabajo, es posible que desee realizar las siguientes operaciones

* Guarde los archivos adjuntos del formulario adaptable en el sistema de archivos
* Manipulación de los datos enviados

Para lograr el caso de uso anterior, normalmente escribirá un servicio OSGi que se ejecuta mediante el paso del proceso.

## Crear proyecto de Maven

El primer paso es crear un proyecto maven utilizando el tipo de archivo Maven de Adobe apropiado. Los pasos detallados se enumeran en esta [article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html). Una vez que haya importado el proyecto maven en eclipse, estará listo para empezar a escribir su primer componente OSGi que se pueda utilizar en el paso del proceso.


### Crear clase que implemente WorkflowProcess

Abra el proyecto maven en su IDE de eclipse. Expandir **projectname** > **core** carpeta. Expanda la carpeta src/main/java. Debería ver un paquete que termina con &quot;core&quot;. Cree una clase Java que implemente WorkflowProcess en este paquete. Deberá anular el método execute . La firma del método execute es la siguiente: public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)lanza WorkflowException El método execute da acceso a las 3 variables siguientes

**WorkItem**: La variable workItem proporcionará acceso a los datos relacionados con el flujo de trabajo. La documentación de la API pública está disponible [aquí.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession**: Esta variable workflowSession le permite controlar el flujo de trabajo. La documentación de la API pública está disponible [here](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap**: Todos los metadatos asociados al flujo de trabajo. Los argumentos de proceso que se pasen al paso de proceso estarán disponibles mediante el objeto MetaDataMap .[Documentación de API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

En este tutorial, se escriben los archivos adjuntos añadidos al formulario adaptable al sistema de archivos como parte del flujo de trabajo AEM.

Para lograr este caso de uso, se escribió la siguiente clase java

Veamos este código

```java
package com.learningaemforms.adobe.core;

import java.io.File;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

import javax.jcr.Node;
import javax.jcr.Session;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.docmanager.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.cq.search.PredicateGroup;
import com.day.cq.search.Query;
import com.day.cq.search.QueryBuilder;
import com.day.cq.search.result.Hit;
import com.day.cq.search.result.SearchResult;

@Component(property = {
	Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
	Constants.SERVICE_VENDOR + "=Adobe Systems",
	"process.label" + "=Save Adaptive Form Attachments to File System"
})
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {

	private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
	@Reference
	QueryBuilder queryBuilder;

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
		Map<String, String> map = new HashMap<String, String> ();
		map.put("path", payloadPath + "/" + attachmentsPath);
		File saveLocationFolder = new File(saveToLocation);
		if (!saveLocationFolder.exists()) {
			saveLocationFolder.mkdirs();
		}

		map.put("type", "nt:file");
		Query query = queryBuilder.createQuery(PredicateGroup.create(map), workflowSession.adaptTo(Session.class));
		query.setStart(0);
		query.setHitsPerPage(20);

		SearchResult result = query.getResult();
		log.debug("Got  " + result.getHits().size() + " attachments ");
		Node attachmentNode = null;
		for (Hit hit: result.getHits()) {
			try {
				String path = hit.getPath();
				log.debug("The attachment title is  " + hit.getTitle() + " and the attachment path is  " + path);
				attachmentNode = workflowSession.adaptTo(Session.class).getNode(path + "/jcr:content");
				InputStream documentStream = attachmentNode.getProperty("jcr:data").getBinary().getStream();
				Document attachmentDoc = new Document(documentStream);
				attachmentDoc.copyToFile(new File(saveLocationFolder + File.separator + hit.getTitle()));
				attachmentDoc.close();
			} catch (Exception e) {
				log.debug("Error saving file " + e.getMessage());
			}
```

Línea 1: define las propiedades de nuestro componente. La propiedad process.label es lo que verá al asociar el componente OSGi con el paso de proceso, como se muestra en una de las capturas de pantalla siguientes.

Líneas 13-15 - Los argumentos de proceso pasados a este componente OSGi se dividen utilizando el separador &quot;,&quot;. Los valores de attachmentPath y saveToLocation se extraen de la matriz de cadenas.

* attachmentPath : es la misma ubicación que especificó en el formulario adaptable cuando configuró la acción de envío del formulario adaptable para invocar AEM flujo de trabajo. Es el nombre de la carpeta en la que desea guardar los archivos adjuntos en AEM en relación con la carga útil del flujo de trabajo.

* saveToLocation : es la ubicación en la que desea guardar los archivos adjuntos en el sistema de archivos del servidor de AEM.

Estos dos valores se pasan como argumentos de proceso como se muestra en la captura de pantalla siguiente.

![ProcessStep](assets/implement-process-step.gif)

El servicio QueryBuilder se utiliza para consultar nodos de tipo nt:file en la carpeta attachmentPath. El resto del código se repite a través de los resultados de búsqueda para crear el objeto Document y guardarlo en el sistema de archivos


>[!NOTE]
>
>Como se usa un objeto Document específico de AEM Forms, es necesario incluir la dependencia aemfd-client-sdk en el proyecto maven. El ID de grupo es com.adobe.aemfd y el id de artefacto es aemfd-client-sdk.

#### Generar e implementar

[Cree el paquete como se describe aquí](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/creating-your-first-osgi-bundle/create-your-first-osgi-bundle.html)
[Asegúrese de que el paquete esté implementado y en estado activo](http://localhost:4502/system/console/bundles)

Cree un modelo de flujo de trabajo. Arrastre y suelte el paso del proceso en el modelo de flujo de trabajo. Asocie el paso del proceso con &quot;Guardar archivos adjuntos de formulario adaptables en el sistema de archivos&quot;.

Proporcione los argumentos de proceso necesarios separados por una coma. Por ejemplo, archivos adjuntos, c:\\scrappp\\. El primer argumento es la carpeta en la que se van a almacenar los archivos adjuntos del formulario adaptable en relación con la carga útil del flujo de trabajo. Debe ser el mismo valor que especificó al configurar la acción de envío del formulario adaptable. El segundo argumento es la ubicación en la que desea que se almacenen los archivos adjuntos.

Creación de un formulario adaptable. Arrastre y suelte el componente Archivos adjuntos en el formulario. Configure la acción de envío del formulario para invocar el flujo de trabajo creado en los pasos anteriores. Proporcione la ruta de conexión adecuada.

Guarde la configuración.

Obtener una vista previa del formulario. Añada un par de archivos adjuntos y envíe el formulario. Los archivos adjuntos deben guardarse en el sistema de archivos en la ubicación especificada por usted en el flujo de trabajo.
