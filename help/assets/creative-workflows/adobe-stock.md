---
title: Uso de recursos de Adobe Stock con AEM Assets
description: 'AEM permite a los usuarios buscar, previsualización, guardar y otorgar licencias de los recursos de Adobe Stock directamente desde AEM. Las organizaciones ahora pueden integrar su plan Adobe Stock Enterprise con AEM Assets para asegurarse de que los recursos con licencia estén ahora ampliamente disponibles para sus proyectos creativos y de marketing, con las potentes funciones de administración de recursos de AEM. '
feature: creative-cloud-integration
topics: authoring, collaboration, operations, sharing, metadata, images, stock
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 6%

---


# Uso de Adobe Stock con AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 ofrece a los usuarios la posibilidad de buscar, previsualización, guardar y obtener licencias de los recursos de Adobe Stock directamente desde AEM. Las organizaciones ahora pueden integrar su plan Adobe Stock Enterprise con AEM Assets para asegurarse de que los recursos con licencia estén ahora ampliamente disponibles para sus proyectos creativos y de marketing, con las potentes funciones de administración de recursos de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>La integración requiere un plan [Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) empresarial y AEM 6.4 con al menos Service Pack 2 implementado. Para obtener más información sobre el Service Pack de AEM 6.4, consulte estas [notas](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)de la versión.

La integración de Adobe Stock y AEM Assets permite a los creadores y comerciantes de contenido obtener fácilmente licencias y utilizar recursos de acciones para fines creativos o de marketing. Puede realizar una búsqueda de recursos de almacenamiento mediante la búsqueda de Omni, agregando el filtro de ubicación como Adobe Stock o navegando a través de la navegación principal de AEM Assets y haciendo clic en el icono de la IU de Adobe Stock Coral.

## Capacidades

### Buscar y guardar

* Realice una búsqueda de recursos de Adobe Stock sin salir de AEM espacio de trabajo.
* Guarde los recursos de Adobe Stock para su previsualización, sin necesidad de otorgar licencias al recurso.
* Capacidad para obtener licencias y guardar recursos de Adobe Stock en AEM Assets
* Posibilidad de buscar recursos similares desde Adobe Stock en la interfaz de usuario de AEM Assets
* Vista de un recurso seleccionado desde la búsqueda de acciones en el sitio web de AEM Assets en Adobe Stock
* Los archivos de recursos con licencia están marcados con una marca azul con licencia para facilitar la identificación

### Metadatos de recurso

* El recurso con licencia se almacena en AEM Assets. Las propiedades del recurso contienen metadatos de almacenamiento en una ficha de metadatos de recurso independiente
* Capacidad para agregar referencias de licencia a metadatos de recursos

### Perfil de stock de activos

* Un usuario puede seleccionar Adobe Stock perfil en *Usuario > Mis preferencias > Configuración de almacenamiento*
* Las referencias obligatorias y opcionales se pueden agregar a la ventana Administración de licencias de recursos.
* Posibilidad de elegir la preferencia de idioma para la ventana de licencias de recursos en función de la región.

### Filtro

* Un usuario puede filtrar los recursos de acciones según el tipo de recurso, la orientación y la Vista similares
* El tipo de recurso incluye fotografías, ilustraciones, vectores, vídeos, plantillas, 3D, Premium, editoriales
* La orientación incluye horizontal, vertical y cuadrada.
* Vista Un filtro similar requiere el número de archivo de Adobe Stock

### Control de acceso

* Los administradores pueden proporcionar permisos a determinados usuarios o grupos para obtener licencias de recursos de acciones al configurar el servicio de nube de Adobe Stock.
* Si un usuario o grupo específico no tiene permiso para otorgar licencias a los activos de acciones, se desactivaría la capacidad de licencias *de recursos de* stock/búsqueda de activos.

## Configurar Adobe Stock con AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 ofrece a los usuarios la posibilidad de buscar, previsualización, guardar y obtener licencias de los recursos de Adobe Stock directamente desde AEM. En este vídeo se explica cómo configurar Adobes con AEM Assets mediante la consola de E/S de Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Para la configuración del servicio de Adobe Stock Cloud, debe seleccionar el punto de ruta del recurso con licencia y Entorno de PROD en /content/dam. El campo entorno se eliminará en la próxima versión de AEM y la ruta de acceso del recurso con licencia formará parte de una función futura y la compatibilidad con este campo se introducirá en la próxima versión de AEM.

>[!NOTE]
>
>La integración requiere un plan [Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) empresarial y AEM 6.4 con al menos [Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) implementado. Para obtener más información sobre el Service Pack de AEM 6.4, consulte estas [notas](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)de la versión. Para configurar la integración, también necesita permisos de administración para [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) y Adobe Experience Manager.

### Instalación {#installations}

* Para AEM 6.4, debe instalar el [AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) y volver a instalar el archivo cq-dam-stock-integration-content-1.0.4.zip.
* Asegúrese de que dispone de permisos de administración en la consola [de E/S de](https://console.adobe.io/)Adobe, [Adobe Admin Console](https://adminconsole.adobe.com/) y Adobe Experience Manager para configurar la integración.

#### Configuración de Adobe IMS mediante la consola de E/S de Adobe {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Creación de una configuración de cuenta técnica de IMS de Adobe en **Herramientas > Seguridad**
2. Seleccione *Cloud Solution* como *Adobe Stock* y cree un nuevo certificado o reutilice un certificado existente para la configuración.
3. Vaya a la consola de E/S de Adobe y cree una nueva integración de cuentas de servicio para *Adobe Stock*.
4. Cargue el certificado del paso 2 en la integración de su cuenta de servicio de Adobe Stock.
5. Elija la configuración de Adobe Stock perfil requerida y complete la integración de servicio.
6. Utilice los detalles de la integración para completar la configuración de la cuenta técnica de IMS de Adobe
7. Asegúrese de que puede recibir el token de acceso mediante la cuenta técnica de IMS de Adobe.

![Cuenta técnica de IMS de Adobe](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurar Cloud Services de Adobe Stock {#set-up-adobe-stock-cloud-services}

1. Cree una nueva configuración de servicio en la nube para Adobe Stock en **Herramientas > Cloud Services.**
2. Seleccione la configuración *de* Adobe IMS creada en la sección anterior para la configuración de *Adobe Stock Cloud*

3. Asegúrese de seleccionar el **ENTORNO** como PROD. El entorno de ensayo no es compatible y se eliminará en la próxima versión de AEM.
4. **La ruta** del recurso con licencia puede apuntar a cualquier directorio en /content/dam. La compatibilidad con funciones de este campo se agregará en la próxima versión de AEM
5. Seleccione la configuración regional y complete la configuración.
6. También puede agregar usuarios o grupos a su servicio de Adobe Stock Cloud para habilitar el acceso para usuarios o grupos específicos.

![Configuración de existencias de Adobe Assets](assets/screen_shot_2018-10-22at12425pm.png)

### Recursos adicionales

* [Plan de acciones empresariales](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notas de la versión de AEM 6.4 Service Pack 2](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Integrar AEM y Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [API de integración de la consola de E/S de Adobe](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentos de API de Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)