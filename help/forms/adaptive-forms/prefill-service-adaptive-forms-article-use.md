---
title: Servicio de relleno previo en formularios adaptables
seo-title: Servicio de relleno previo en formularios adaptables
description: Rellenar previamente formularios adaptables recuperando datos de orígenes de datos back-end.
seo-description: Rellenar previamente formularios adaptables recuperando datos de orígenes de datos back-end.
sub-product: formularios
feature: Formularios adaptables
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 1%

---


# Uso del servicio de relleno previo en formularios adaptables

Puede rellenar previamente los campos de un formulario adaptable utilizando los datos existentes. Cuando un usuario abre un formulario, los valores de esos campos se rellenan previamente. Existen varias formas de rellenar previamente los campos de formularios adaptables. En este artículo, analizaremos el prefijo del formulario adaptable mediante el servicio de prerellenado de AEM Forms.

Para obtener más información sobre los distintos métodos para rellenar previamente formularios adaptables, [siga esta documentación](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Para rellenar previamente un formulario adaptable mediante el servicio de rellenado previo, deberá crear una clase que implemente la interfaz DataProvider . El método getPrefillData tendrá la lógica para generar y devolver datos que el formulario adaptable consumirá para rellenar previamente los campos. En este método, puede recuperar los datos de cualquier origen y devolver el flujo de entrada del documento de datos. El siguiente código de ejemplo obtiene la información de perfil de usuario del usuario que ha iniciado sesión y construye un documento XML cuya secuencia de entrada se devuelve para que los formularios adaptables la consuman.

En el siguiente fragmento de código tenemos una clase que implementa la interfaz DataProvider. Obtenemos acceso al usuario que ha iniciado sesión y, a continuación, recuperamos la información de perfil del usuario que ha iniciado sesión. A continuación, creamos un documento XML con un elemento de nodo raíz llamado &quot;data&quot; y anexamos los elementos adecuados a este nodo de datos. Una vez construido el documento XML, se devuelve el flujo de entrada del documento XML.

Esta clase se integra en el paquete OSGi y se implementa en AEM. Una vez implementado el paquete, este servicio de rellenado previo está disponible para ser utilizado como servicio de rellenado previo del formulario adaptable.

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

Para probar esta capacidad en el servidor, realice lo siguiente

* [Descargue y extraiga el contenido del archivo zip en su equipo](assets/prefillservice.zip)
* Asegúrese de que la información del perfil del usuario [usuario](http://localhost:4502/libs/granite/security/content/useradmin) que ha iniciado sesión se rellena por completo. Este es un requisito para que funcione la muestra. El ejemplo no tiene ninguna comprobación de errores para comprobar si faltan propiedades de perfil de usuario.
* Implemente el paquete utilizando la [consola web de AEM](http://localhost:4502/system/console/bundles)
* Creación de formularios adaptables mediante el XSD
* Asocie &quot;Servicio de prerellenado de formularios personalizados de Aem&quot; como servicio de precumplimentación del formulario adaptable
* Arrastrar y soltar elementos de esquema en el formulario
* Obtener una vista previa del formulario

>[!NOTE]
>
>Si el formulario adaptable se basa en XSD, asegúrese de que el documento XML devuelto por el servicio de rellenado previo coincide con el XSD en el que se basa el formulario adaptable.
>
>Si el formulario adaptable no se basa en XSD, tendrá que enlazar manualmente los campos. Por ejemplo, para enlazar un campo de formulario adaptable al elemento fname en los datos XML, utilice `/data/fname` en la referencia Enlace del campo de formulario adaptable.

