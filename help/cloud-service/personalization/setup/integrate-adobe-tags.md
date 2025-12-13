---
title: Integración de etiquetas en Adobe Experience Platform
description: Obtenga información sobre cómo integrar AEM as a Cloud Service con etiquetas en Adobe Experience Platform. La integración de permite implementar Adobe Web SDK e insertar JavaScript personalizado para la recopilación y personalización de datos en las páginas de AEM.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture, Content Management
role: Developer, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18719
thumbnail: null
exl-id: 71cfb9f5-57d9-423c-bd2a-f6940cc0b4db
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 1%

---

# Integración de etiquetas en Adobe Experience Platform

Obtenga información sobre cómo integrar AEM as a Cloud Service (AEM CS) con etiquetas en Adobe Experience Platform. La integración de etiquetas (también conocida como Launch) le permite implementar Adobe Web SDK e insertar JavaScript personalizado para la recopilación y personalización de datos en sus páginas de AEM.

La integración permite a su equipo de marketing o desarrollo administrar e implementar JavaScript para la personalización y la recopilación de datos, sin necesidad de volver a implementar el código de AEM.

## Pasos de alto nivel

El proceso de integración implica cuatro pasos principales que establecen la conexión entre AEM y las etiquetas:

1. **Crear, configurar y publicar una propiedad Tags en Adobe Experience Platform**
2. **Verificar una configuración de Adobe IMS para etiquetas en AEM**
3. **Crear una configuración de etiquetas en AEM**
4. **Aplicar la configuración de etiquetas a las páginas de AEM**

## Creación, configuración y publicación de una propiedad de etiquetas en Adobe Experience Platform

Comience creando una propiedad Tags en Adobe Experience Platform. Esta propiedad le ayuda a administrar la implementación de Adobe Web SDK y cualquier JavaScript personalizado que se requiera para la personalización y la recopilación de datos.

1. Ve a [Adobe Experience Platform](https://experience.adobe.com/platform), inicia sesión con tu Adobe ID y navega a **Etiquetas** desde el menú de la izquierda.\
   ![Etiquetas de Adobe Experience Platform](../assets/setup/aep-tags.png)

2. Haga clic en **Nueva propiedad** para crear una nueva propiedad de etiquetas.\
   ![Crear nueva propiedad de etiquetas](../assets/setup/aep-create-tags-property.png)

3. En el cuadro de diálogo **Crear propiedad**, escriba lo siguiente:
   - **Nombre de propiedad**: Un nombre para su propiedad de etiquetas
   - **Tipo de propiedad**: Seleccionar **Web**
   - **Dominio**: Dominio donde implementa la propiedad (por ejemplo, `.adobeaemcloud.com`)

   Haga clic en **Guardar**.

   ![Propiedad de etiquetas de Adobe](../assets/setup/adobe-tags-property.png)

4. Abra la nueva propiedad. La extensión **Core** ya debería estar incluida. Más adelante, agregará la extensión **Web SDK** al configurar el caso de uso Experimentación, ya que requiere una configuración adicional como **Datastream ID**.\
   ![Extensión principal de etiquetas de Adobe](../assets/setup/adobe-tags-core-extension.png)

5. Publique la propiedad Tags yendo a **Flujo de publicación** y haciendo clic en **Agregar biblioteca** para crear una biblioteca de implementación.
   ![Flujo de publicación de etiquetas de Adobe](../assets/setup/adobe-tags-publishing-flow.png)

6. En el cuadro de diálogo **Crear biblioteca**, proporcione:
   - **Nombre**: Un nombre para su biblioteca
   - **Entorno**: Seleccione **Desarrollo**
   - **Cambios de recursos**: Elija **Agregar todos los recursos modificados**

   Haga clic en **Guardar y generar en desarrollo**.

   ![Biblioteca de creación de etiquetas de Adobe](../assets/setup/adobe-tags-create-library.png)

7. Para publicar la biblioteca en producción, haga clic en **Aprobar y publicar en producción**. Una vez finalizada la publicación, la propiedad está lista para utilizarse en AEM.\
   ![Etiquetas Adobe: Aprobar y publicar](../assets/setup/adobe-tags-approve-publish.png)

## Verificación de una configuración de IMS de Adobe para etiquetas en AEM

Cuando se aprovisiona un entorno de AEM CS, se incluye automáticamente una configuración IMS de Adobe para las etiquetas, junto con un proyecto de Adobe Developer Console correspondiente. Esta configuración garantiza la comunicación segura de la API entre AEM y las etiquetas.

1. En AEM, vaya a **Herramientas** > **Seguridad** > **Configuraciones de Adobe IMS**.\
   ![Configuraciones de Adobe IMS](../assets/setup/aem-ims-configurations.png)

2. Busque la configuración de **Adobe Launch**. Si está disponible, selecciónelo y haga clic en **Comprobar estado** para comprobar la conexión. Debería ver una respuesta de éxito.\
   ![Comprobación de estado de la configuración de Adobe IMS](../assets/setup/aem-ims-configuration-health-check.png)

## Creación de una configuración de etiquetas en AEM

Cree una configuración de Etiquetas en AEM para especificar la propiedad y la configuración necesarias para las páginas del sitio.

1. En AEM, vaya a **Herramientas** > **Cloud Services** > **Configuraciones de Adobe Launch**.\
   ![Configuraciones de Adobe Launch](../assets/setup/aem-launch-configurations.png)

2. Seleccione la carpeta raíz del sitio (por ejemplo, WKND Site) y haga clic en **Crear**.\
   ![Crear configuración de Adobe Launch](../assets/setup/aem-create-launch-configuration.png)

3. En el cuadro de diálogo, introduzca lo siguiente:
   - **Título**: Por ejemplo, &quot;Etiquetas Adobe&quot;
   - **Configuración de IMS**: seleccione la configuración verificada de **Adobe Launch** IMS
   - **Empresa**: seleccione la empresa vinculada a su propiedad de etiquetas
   - **Propiedad**: elija la propiedad Etiquetas creada anteriormente

   Haga clic en **Siguiente**.

   ![Detalles de configuración de Adobe Launch](../assets/setup/aem-launch-configuration-details.png)

4. Para fines de demostración, mantenga los valores predeterminados de los entornos **Staging** y **Production**. Haga clic en **Crear**.\
   ![Detalles de configuración de Adobe Launch](../assets/setup/aem-launch-configuration-create.png)

5. Seleccione la configuración recién creada y haga clic en **Publicar** para que esté disponible en las páginas del sitio.\
   ![Publicación de configuración de Adobe Launch](../assets/setup/aem-launch-configuration-publish.png)

## Aplicación de la configuración de etiquetas a su sitio de AEM

Aplique la configuración de etiquetas para insertar la lógica de SDK web y de personalización en las páginas del sitio.

1. En AEM, ve a **Sitios**, selecciona la carpeta raíz del sitio (por ejemplo, Sitio WKND) y haz clic en **Propiedades**.\
   ![Propiedades del sitio AEM](../assets/setup/aem-site-properties.png)

2. En el cuadro de diálogo **Propiedades del sitio**, abra la ficha **Avanzadas**. En **Configuraciones**, asegúrese de que `/conf/wknd` esté seleccionado para **Configuración de nube**.\
   ![Propiedades avanzadas del sitio AEM](../assets/setup/aem-site-advanced-properties.png)

## Verificar la integración

Para confirmar que la configuración de Etiquetas funciona correctamente, puede:

1. Compruebe el origen de la vista de una página de publicación de AEM o inspecciónela con las herramientas para desarrolladores del explorador
2. Use [Adobe Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) para validar Web SDK y la inyección de JavaScript

![Adobe Experience Platform Debugger](../assets/setup/aep-debugger.png)

## Recursos adicionales

- [Información general de Adobe Experience Platform Debugger](https://experienceleague.adobe.com/es/docs/experience-platform/debugger/home)
- [Información general sobre las etiquetas](https://experienceleague.adobe.com/es/docs/experience-platform/tags/home)
