---
title: Integración con Adobe Target
description: Obtenga información sobre cómo integrar AEM as a Cloud Service con Adobe Target para administrar y activar contenido personalizado (fragmentos de experiencias) como ofertas.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18718
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 2%

---


# Integración con Adobe Target

Obtenga información sobre cómo integrar AEM as a Cloud Service (AEM CS) con Adobe Target para activar contenido personalizado, como fragmentos de experiencias, como ofertas en Adobe Target.

La integración permite a su equipo de marketing crear y administrar contenido personalizado de forma centralizada en AEM. Este contenido se puede activar fácilmente como ofertas en Adobe Target.

>[!IMPORTANT]
>
>El paso de integración es opcional si su equipo prefiere administrar las ofertas completamente dentro de Adobe Target, sin utilizar AEM como repositorio de contenido centralizado.

## Pasos de alto nivel

El proceso de integración implica cuatro pasos principales que establecen la conexión entre AEM y Adobe Target:

1. **Crear y configurar un proyecto de Adobe Developer Console**
2. **Crear una configuración de Adobe IMS para Target en AEM**
3. **Crear una configuración de Adobe Target heredada en AEM**
4. **Aplicar la configuración de Adobe Target a los fragmentos de experiencias**

## Creación y configuración de un proyecto de Adobe Developer Console

Para permitir que AEM se comunique de forma segura con Adobe Target, debe configurar un proyecto de Adobe Developer Console mediante la autenticación de servidor a servidor OAuth. Puede utilizar un proyecto existente o crear uno nuevo.

1. Ve a [Adobe Developer Console](https://developer.adobe.com/console) e inicia sesión con tu Adobe ID.

2. Cree un nuevo proyecto o seleccione uno existente.\
   ![Proyecto Adobe Developer Console](../assets/setup/adc-project.png)

3. Haga clic en **Agregar API**. En el cuadro de diálogo **Agregar una API**, filtre por **Experience Cloud**, seleccione **Adobe Target** y haga clic en **Siguiente**.\
   ![Agregar API al proyecto](../assets/setup/adc-add-api.png)

4. En el cuadro de diálogo **Configurar API**, seleccione el método de autenticación **Servidor a servidor OAuth** y haga clic en **Siguiente**.\
   ![Configurar API](../assets/setup/adc-configure-api.png)

5. En el paso **Seleccionar perfiles de producto**, seleccione la **Workspace predeterminada** y haga clic en **Guardar la API configurada**.\
   ![Seleccionar perfiles de producto](../assets/setup/adc-select-product-profiles.png)

6. En el panel de navegación de la izquierda, seleccione **Servidor a servidor OAuth** y revise los detalles de configuración. Tenga en cuenta el ID de cliente y el Secreto del cliente: necesita estos valores para configurar la integración de IMS en AEM.
   ![Detalles de servidor a servidor de OAuth](../assets/setup/adc-oauth-server-to-server.png)

## Creación de una configuración de Adobe IMS para Target en AEM

En AEM, cree una configuración de IMS de Adobe para Target con las credenciales de Adobe Developer Console. Esta configuración permite a AEM autenticarse con las API de Adobe Target.

1. En AEM, vaya a **Herramientas** > **Seguridad** y seleccione **Configuraciones de Adobe IMS**.\
   ![Configuraciones de Adobe IMS](../assets/setup/aem-ims-configurations.png)

2. Haga clic en **Crear**.\
   ![Crear configuración de Adobe IMS](../assets/setup/aem-create-ims-configuration.png)

3. En la página **Configuración de cuenta técnica de Adobe IMS**, escriba lo siguiente:
   - **Solución de nube**: Adobe Target
   - **Título**: Una etiqueta para la configuración, como &quot;Adobe Target&quot;
   - **Servidor de autorización**: `https://ims-na1.adobelogin.com`
   - **ID de cliente**: desde Adobe Developer Console
   - **Secreto de cliente**: de Adobe Developer Console
   - **Ámbito**: desde Adobe Developer Console
   - **ID de organización**: de Adobe Developer Console

   Luego haga clic en **Crear**.

   ![Detalles de configuración de Adobe IMS](../assets/setup/aem-ims-configuration-details.png)

4. Seleccione la configuración y haga clic en **Comprobar estado** para comprobar la conexión. Un mensaje de éxito confirma que AEM se puede conectar a Adobe Target.\
   ![Comprobación de estado de la configuración de Adobe IMS](../assets/setup/aem-ims-configuration-health-check.png)

## Creación de una configuración de Adobe Target heredada en AEM

Para exportar fragmentos de experiencias como ofertas a Adobe Target, cree una configuración de Adobe Target heredada en AEM.

1. En AEM, vaya a **Herramientas** > **Cloud Services** y seleccione **Cloud Services heredados**.\
   ![Servicios de nube heredados](../assets/setup/aem-legacy-cloud-services.png)

2. En la sección **Adobe Target**, haga clic en **Configurar ahora**.\
   ![Configurar elementos heredados de Adobe Target](../assets/setup/aem-configure-adobe-target-legacy.png)

3. En el cuadro de diálogo **Crear configuración**, escriba un nombre como &quot;Adobe Target heredado&quot; y haga clic en **Crear**.\
   ![Crear configuración heredada de Adobe Target](../assets/setup/aem-create-adobe-target-legacy-configuration.png)

4. En la página **Configuración heredada de Adobe Target**, proporcione lo siguiente:
   - **Autenticación**: IMS
   - **Código de cliente**: Su código de cliente de Adobe Target (encontrado en Adobe Target en **Administración** > **Implementación**)
   - **Configuración de IMS**: La configuración de IMS que creó anteriormente

   Haga clic en **Conectarse a Adobe Target** para validar la conexión.

   ![Configuración heredada de Adobe Target](../assets/setup/aem-target-legacy-configuration.png)

## Aplicar la configuración de Adobe Target a los fragmentos de experiencias

Asocie la configuración de Adobe Target a los fragmentos de experiencias para que se puedan exportar y utilizar como ofertas en Target.

1. En AEM, vaya a **Fragmentos de experiencias**.\
   ![Fragmentos de experiencias](../assets/setup/aem-experience-fragments.png)

2. Seleccione la carpeta raíz que contiene los fragmentos de experiencias (por ejemplo, `WKND Site Fragments`) y haga clic en **Propiedades**.\
   ![Propiedades de fragmentos de experiencias](../assets/setup/aem-experience-fragments-properties.png)

3. En la página **Propiedades**, abra la pestaña **Cloud Services**. En la sección **Configuraciones de Cloud Service**, seleccione la configuración de Adobe Target.\
   ![Servicios de Experience Fragments Cloud](../assets/setup/aem-experience-fragments-cloud-services.png)

4. En la sección **Adobe Target** que aparece, complete lo siguiente:
   - **Formato de exportación de Adobe Target**: HTML
   - **Adobe Target Workspace**: seleccione el área de trabajo que desea utilizar (por ejemplo, &quot;Workspace predeterminado&quot;)
   - **Dominios externalizadores**: escriba los dominios para generar direcciones URL externas

   ![Configuración de Adobe Target de fragmentos de experiencias](../assets/setup/aem-experience-fragments-adobe-target-configuration.png)

5. Haga clic en **Guardar y cerrar** para aplicar la configuración.

## Verificar la integración

Para confirmar que la integración funciona correctamente, pruebe la funcionalidad de exportación:

1. En AEM, cree un nuevo fragmento de experiencia o abra uno existente. Haga clic en **Exportar a Adobe Target** en la barra de herramientas.\
   ![Exportar fragmento de experiencia a Adobe Target](../assets/setup/aem-export-experience-fragment-to-adobe-target.png)

2. En Adobe Target, vaya a la sección **Ofertas** y compruebe que el Fragmento de experiencia aparece como una oferta.\
   ![Ofertas de Adobe Target](../assets/setup/adobe-target-xf-as-offer.png)

## Recursos adicionales

- [Resumen de API de Target](https://experienceleague.adobe.com/es/docs/target-dev/developer/api/target-api-overview)
- [Oferta de Target](https://experienceleague.adobe.com/es/docs/target/using/experiences/offers/manage-content)
- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/)
- [Fragmentos de experiencias en AEM](https://experienceleague.adobe.com/es/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use)