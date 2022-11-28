---
title: Servicio de precarga en Forms adaptable
description: Rellenar previamente formularios adaptables recuperando datos de orígenes de datos back-end.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: 381812397fa7d15f6ee34ef85ddf0aa0acc0af42
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 3%

---

# Uso del servicio Prefill en Forms adaptable

Puede rellenar previamente los campos de un formulario adaptable utilizando los datos existentes. Cuando un usuario abre un formulario, los valores de esos campos están rellenos previamente. Existen varias formas de rellenar previamente los campos de formularios adaptables. En este artículo, analizaremos el prefiriendo formularios adaptables con el servicio de prellenado de AEM Forms.

Para obtener más información sobre los distintos métodos para rellenar previamente formularios adaptables, [siga esta documentación](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Para rellenar previamente un formulario adaptable mediante el servicio de rellenado previo, debe crear una clase que implemente la variable `com.adobe.forms.common.service.DataXMLProvider` interfaz. El método `getDataXMLForDataRef` tendrá la lógica para generar y devolver datos que el formulario adaptable utilizará para rellenar previamente los campos. En este método, puede recuperar los datos de cualquier origen y devolver el flujo de entrada del documento de datos. El siguiente código de ejemplo obtiene la información de perfil de usuario del usuario que ha iniciado sesión y construye un documento XML cuya secuencia de entrada se devuelve para que la consuman los formularios adaptables.

En el fragmento de código siguiente tenemos una clase que implementa la interfaz DataXMLProvider. Obtenemos acceso al usuario que ha iniciado sesión y, a continuación, recuperamos la información de perfil del usuario que ha iniciado sesión. A continuación, creamos un documento XML con un elemento de nodo raíz llamado &quot;data&quot; y anexamos los elementos adecuados a este nodo de datos. Una vez construido el documento XML, se devuelve el flujo de entrada del documento XML.

A continuación, esta clase se convierte en un paquete OSGi y se implementa en AEM. Una vez implementado el paquete, este servicio de rellenado previo está disponible para ser utilizado como servicio de rellenado previo del formulario adaptable.

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

* Asegúrese de que ha iniciado sesión [perfil del usuario](http://localhost:4502/security/users.html) se rellena. El ejemplo busca las propiedades FirstName, LastName y Email del usuario que ha iniciado sesión.
* [Descargue y extraiga el contenido del archivo zip en su equipo](assets/prefillservice.zip)
* Implemente el paquete prefill.core-1.0.0-SNAPSHOT utilizando el [consola web AEM](http://localhost:4502/system/console/bundles)
* Importar el formulario adaptable mediante la opción Crear | Carga de archivos desde el [Sección FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Asegúrese de que la variable [formulario](http://localhost:4502/editor.html/content/forms/af/prefill.html) está utilizando **&quot;Servicio de prerellenado personalizado de AEM Forms&quot;** como servicio de precarga. Esto se puede comprobar desde las propiedades de configuración de la variable **Contenedor de formulario** para obtener más información.
* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). Debería ver el formulario rellenado con los valores correctos.

>[!NOTE]
>
>Si ha habilitado la depuración para com.aem.prefill.core.PrefillAdaptiveForm, el archivo de datos xml generado se escribe en la carpeta de instalación del servidor de AEM.

