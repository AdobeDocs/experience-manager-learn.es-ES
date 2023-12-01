---
title: Generación de DoR interactivo con datos de formulario adaptable
description: Combinar datos de formulario adaptables con la plantilla XDP para generar un PDF descargable
version: 6.4,6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 1%

---

# Descargar documento de registro interactivo

Un caso de uso común es poder descargar un DoR interactivo con los datos del formulario adaptable. El DoR descargado se completará usando Adobe Acrobat o Adobe Reader.

## El formulario adaptable no se basa en el esquema XSD

Si el XDP y el formulario adaptable no están basados en ningún esquema, siga los siguientes pasos para generar un documento de registro interactivo.

### Crear formulario adaptable

Cree un formulario adaptable y asegúrese de que los nombres de los campos del formulario adaptable sean idénticos a los nombres de los campos de la plantilla xdp.
Anote el nombre del elemento raíz de la plantilla xdp.
![elemento-raíz](assets/xfa-root-element.png)

### Biblioteca de cliente

El siguiente código se activa cuando se activa el botón Descargar PDF

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
                req.onreadystatechange = function() {
                    if (req.readyState == 4 && req.status == 200) {
                        download(this.response, "report.pdf", "application/pdf");
                    }


                }
            }
        });

    });
});
```

## Formulario adaptable basado en el esquema XSD

Si su xdp no se basa en XSD, siga los siguientes pasos para crear el XSD (esquema) en el que basará su formulario adaptable

### Generar datos de ejemplo para el XDP

* Abra el XDP en AEM Forms Designer.
* Haga clic en Archivo | Propiedades del formulario | Vista previa
* Haga clic en Generar datos de previsualización
* Haga clic en Generar
* Proporcionar un nombre de archivo significativo como &quot;form-data.xml&quot;

### Generar XSD a partir de los datos xml

Puede utilizar cualquiera de las herramientas gratuitas en línea para [generar XSD](https://www.freeformatter.com/xsd-generator.html) a partir de los datos xml generados en el paso anterior.

### Crear formulario adaptable

Cree un formulario adaptable basado en el XSD del paso anterior. Asocie el formulario para utilizar la biblioteca de cliente &quot;irs&quot;. Esta biblioteca de cliente tiene el código para realizar una llamada al POST al servlet que devuelve el PDF a la aplicación que realiza la llamada. El siguiente código se activa cuando se activa la función _Descargar PDF_ se hace clic

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
                req.onreadystatechange = function() {
                    if (req.readyState == 4 && req.status == 200) {
                        download(this.response, "report.pdf", "application/pdf");
                    }


                }
            }
        });

    });
});
```



## Crear servlet personalizado

Cree un servlet personalizado que combine los datos con la plantilla XDP y devuelva el PDF. El código para lograrlo se enumera a continuación. El servlet personalizado forma parte del [Paquete AEMFormsDocumentServices.core-1.0-SNAPSHOT](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)).

```java
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
            response.setContentLength((int) fileInputStream.available());
            OutputStream responseOutputStream = response.getOutputStream();
            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }
            responseOutputStream.flush();
            responseOutputStream.close();

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

En el código de ejemplo, extraemos el nombre xdp y otros parámetros del objeto de solicitud. Si el formulario no está basado en XSD, se crea el documento xml que se combinará con el xdp. Si el formulario se basa en XSD, simplemente extraemos el nodo correspondiente de los datos enviados del formulario adaptable para generar el documento xml que se combinará con la plantilla xdp.

## Implementar el ejemplo en el servidor

Para probar esto en el servidor local, siga los siguientes pasos:

1. [Descargar e instalar el paquete DevelopersWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Agregue la siguiente entrada en el servicio de asignador de usuarios del servicio Apache Sling DesarrolloConUsuarioServicio.core:getformsresourceresolver=fd-service
1. [Descargar e instalar el paquete personalizado de Document Services](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Tiene el servlet para combinar los datos con la plantilla XDP y transmitir el PDF de vuelta
1. [Importar la biblioteca de cliente](assets/generate-interactive-dor-client-lib.zip)
1. [Importar los recursos del artículo (formulario adaptable, plantillas XDP y XSD)](assets/generate-interactive-dor-sample-assets.zip)
1. [Previsualizar formulario adaptable](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. Rellene algunos de los campos del formulario.
1. Haga clic en Descargar PDF para obtener el PDF. Es posible que tenga que esperar unos segundos para que el PDF descargue.

>[!NOTE]
>
>Puede probar el mismo caso de uso con [formulario adaptable no basado en xsd](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled). Asegúrese de pasar los parámetros adecuados al extremo posterior en streampdf.js, ubicado en irs clientlib.
