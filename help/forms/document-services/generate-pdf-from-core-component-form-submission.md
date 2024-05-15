---
title: Generar un PDF con datos de un formulario adaptable basado en componentes principales
description: Combine datos del envío de formularios basados en componentes principales con la plantilla XDP en el flujo de trabajo
version: 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-15025
last-substantial-update: 2024-02-26T00:00:00Z
exl-id: cae160f2-21a5-409c-942d-53061451b249
duration: 97
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 0%

---

# Generar PDF con datos del envío de formularios basados en componentes principales

Este es el texto revisado con &quot;Componentes principales&quot; en mayúsculas:

Un escenario típico implica generar un PDF a partir de los datos enviados mediante un formulario adaptable basado en componentes principales. Estos datos siempre están en formato JSON. Para generar un PDF mediante la API del PDF de procesamiento, es necesario convertir los datos JSON al formato XML. El `toString` método de `org.json.XML` se utiliza para esta conversión. Para obtener más información, consulte la [documentación de `org.json.XML.toString` método](https://www.javadoc.io/doc/org.json/json/20171018/org/json/XML.html#toString-java.lang.Object-).

## Formulario adaptable basado en esquema JSON

Siga estos pasos para crear un esquema JSON para su formulario adaptable:

### Generar datos de ejemplo para el XDP

Para racionalizar el proceso, siga estos pasos refinados:

1. Abra el archivo XDP en AEM Forms Designer.
1. Vaya a &quot;Archivo&quot; > &quot;Propiedades del formulario&quot; > &quot;Vista previa&quot;.
1. Seleccione &quot;Generate Preview Data&quot;.
1. Haga clic en &quot;Generar&quot;.
1. Asigne un nombre de archivo significativo, como `form-data.xml`.

### Generar un esquema JSON a partir de los datos XML

Puede utilizar cualquier herramienta en línea gratuita para [convertir XML a JSON](https://jsonformatter.org/xml-to-jsonschema) utilizando los datos XML generados en el paso anterior.

### Proceso de flujo de trabajo personalizado para convertir JSON a XML

El código proporcionado convierte JSON en XML y almacena el XML resultante en una variable de proceso de flujo de trabajo denominada `dataXml`.

```java
import org.slf4j.LoggerFactory;
import com.adobe.granite.workflow.WorkflowException;
import java.io.InputStream;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import javax.jcr.Node;
import javax.jcr.Session;
import org.json.JSONObject;
import org.json.XML;
import org.slf4j.Logger;
import org.osgi.service.component.annotations.Component;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;

@Component(property = {
    "service.description=Convert JSON to XML",
    "process.label=Convert JSON to XML"
})
public class ConvertJSONToXML implements WorkflowProcess {

    private static final Logger log = LoggerFactory.getLogger(ConvertJSONToXML.class);

    @Override
    public void execute(final WorkItem workItem, final WorkflowSession workflowSession, final MetaDataMap arg2) throws WorkflowException {
        String processArgs = arg2.get("PROCESS_ARGS", "string");
        log.debug("The process argument I got was " + processArgs);
        
        String submittedDataFile = processArgs;
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
        log.debug("The payload in convert json to xml " + payloadPath);
        
        String dataFilePath = payloadPath + "/" + submittedDataFile + "/jcr:content";
        try {
            Session session = workflowSession.adaptTo(Session.class);
            Node submittedJsonDataNode = session.getNode(dataFilePath);
            InputStream jsonDataStream = submittedJsonDataNode.getProperty("jcr:data").getBinary().getStream();
            BufferedReader streamReader = new BufferedReader(new InputStreamReader(jsonDataStream, "UTF-8"));
            StringBuilder stringBuilder = new StringBuilder();
            String inputStr;
            while ((inputStr = streamReader.readLine()) != null) {
                stringBuilder.append(inputStr);
            }
            JSONObject submittedJson = new JSONObject(stringBuilder.toString());
            log.debug(submittedJson.toString());
            
            String xmlString = XML.toString(submittedJson);
            log.debug("The json converted to XML " + xmlString);
            
            MetaDataMap metaDataMap = workItem.getWorkflow().getWorkflowData().getMetaDataMap();
            metaDataMap.put("xmlData", xmlString);
        } catch (Exception e) {
            log.error("Error converting JSON to XML: " + e.getMessage(), e);
        }
    }
}
```

### Crear flujo de trabajo

Para gestionar los envíos de formularios, cree un flujo de trabajo que incluya dos pasos:

1. El paso inicial emplea un proceso personalizado para transformar los datos JSON enviados en XML.
1. El paso siguiente genera un PDF al combinar los datos XML con la plantilla XDP.

![json-to-xml](assets/json-to-xml-process-step.png)


## Implemente el código de ejemplo

Para probar esto en el servidor local, siga estos pasos optimizados:

1. [AEM Descargue e instale el paquete personalizado a través de la consola web de OSGi de la](assets/convertJsonToXML.core-1.0.0-SNAPSHOT.jar).
1. [Importación del paquete de flujo de trabajo](assets/workflow_to_render_pdf.zip).
1. [Importe el formulario adaptable y la plantilla XDP de ejemplo](assets/adaptive_form_and_xdp_template.zip).
1. [Previsualización del formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/f23/jcr:content?wcmmode=disabled).
1. Rellene algunos campos de formulario.
1. AEM Envíe el formulario para iniciar el flujo de trabajo de.
1. Busque el PDF procesado en la carpeta de carga útil del flujo de trabajo.
