---
title: Servicio de relleno previo en Forms adaptable
description: Rellene previamente formularios adaptables recuperando datos de fuentes de datos back-end.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: cf37afeb9bea65b540c9cfde75070d4106a01976
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 4%

---

# Uso del servicio de relleno previo en Forms adaptable

Puede rellenar previamente los campos de un formulario adaptable mediante los datos existentes. Cuando un usuario abre un formulario, los valores de esos campos ya han sido rellenados. Existen varias formas de rellenar previamente los campos de formulario adaptables. En este artículo, veremos cómo rellenar previamente el formulario adaptable mediante el servicio de relleno previo de AEM Forms.

Para obtener más información sobre los distintos métodos para rellenar previamente formularios adaptables, [siga esta documentación](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Para rellenar previamente un formulario adaptable mediante el servicio de relleno previo, debe crear una clase que implemente el `com.adobe.forms.common.service.DataXMLProvider` interfaz. El método `getDataXMLForDataRef` tendrá la lógica para generar y devolver los datos que consumirá el formulario adaptable para rellenar previamente los campos. Con este método, puede recuperar los datos de cualquier origen y devolver la secuencia de entrada del documento de datos. El siguiente código de ejemplo recupera la información de perfil del usuario que ha iniciado sesión y construye un documento XML cuya secuencia de entrada devuelve para que la consuman los formularios adaptables.

En el siguiente fragmento de código tenemos una clase que implementa la interfaz DataXMLProvider. Obtenemos acceso al usuario que ha iniciado sesión y, a continuación, recuperamos la información de perfil del usuario que ha iniciado sesión. A continuación, creamos un documento XML con un elemento de nodo raíz denominado &quot;data&quot; y anexamos los elementos adecuados a este nodo de datos. Una vez construido el documento XML, se devuelve la secuencia de entrada del documento XML.

AEM A continuación, esta clase se convierte en un paquete OSGi y se implementa en la interfaz de usuario de. Una vez implementado el paquete, este servicio de rellenado previo está disponible para utilizarse como servicio de rellenado previo del formulario adaptable.

>[!NOTE]
>
>Puede rellenar previamente el formulario utilizando datos xml o json mediante el método que se indica en este artículo

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

Para probar esta capacidad en el servidor, realice lo siguiente

* Asegúrese de que el usuario ha iniciado sesión [perfil del usuario](http://localhost:4502/security/users.html) se rellena la información. El ejemplo busca las propiedades FirstName, LastName y Email del usuario que ha iniciado sesión.
* [Descargue y extraiga el contenido del archivo zip en su ordenador](assets/prefillservice.zip)
* Implemente el paquete prefill.core-1.0.0-SNAPSHOT con el [AEM consola web de](http://localhost:4502/system/console/bundles)
* Importe el formulario adaptable mediante la opción Crear | Carga de archivos desde [Sección Formularios y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Asegúrese de que la [formulario](http://localhost:4502/editor.html/content/forms/af/prefill.html) está utilizando **&quot;Servicio de prerrellenado de AEM Forms personalizado&quot;** como servicio de relleno previo. Esto se puede comprobar desde las propiedades de configuración del **Contenedor del formulario** sección.
* [Previsualización del formulario](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). Debería ver el formulario rellenado con los valores correctos.

>[!NOTE]
>
>AEM Si ha habilitado la depuración para com.aem.prefill.core.PrefillAdaptiveForm, el archivo de datos xml generado se escribirá en la carpeta de instalación del servidor de la.

