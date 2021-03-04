---
title: Uso de recursos de Adobe Stock con AEM Assets
description: 'AEM permite a los usuarios buscar, previsualizar, guardar y conceder licencias sobre los recursos de Adobe Stock directamente desde AEM. Las organizaciones ahora pueden integrar su plan empresarial de Adobe Stock con AEM Assets para garantizar que los recursos con licencia estén ahora disponibles para sus proyectos creativos y de marketing, con las potentes funciones de administración de recursos de AEM. '
feature: Adobe Stock
version: 6.4, 6.5
topic: Administración de contenido
role: Profesional empresarial
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '973'
ht-degree: 6%

---


# Uso de Adobe Stock con AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 proporciona a los usuarios la capacidad de buscar, previsualizar, guardar y licenciar recursos de Adobe Stock directamente desde AEM. Las organizaciones ahora pueden integrar su plan empresarial de Adobe Stock con AEM Assets para garantizar que los recursos con licencia estén ahora disponibles para sus proyectos creativos y de marketing, con las potentes funciones de administración de recursos de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=9&learn=on)

>[!NOTE]
>
>La integración requiere un [plan empresarial de Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) y AEM 6.4 con al menos el Service Pack 2 implementado. Para obtener más información sobre el paquete de servicio de AEM 6.4, consulte estas [notas de la versión](https://helpx.adobe.com/es/experience-manager/6-4/release-notes/sp-release-notes.html).

La integración de Adobe Stock y AEM Assets permite a los autores y especialistas en marketing de contenido obtener fácilmente licencias y utilizar los recursos de existencias para fines creativos o de marketing. Puede realizar una búsqueda de recursos de existencias mediante la búsqueda Omni, agregando el filtro de ubicación como Adobe Stock o navegando por la navegación principal de AEM Assets y haciendo clic en el icono de la interfaz de usuario Buscar de Adobe Stock Coral .

## Capacidades

### Buscar y guardar

* Realice la búsqueda de recursos de Adobe Stock sin salir del espacio de trabajo de AEM.
* Guarde los recursos de Adobe Stock para previsualizarlos, sin necesidad de conceder licencias al recurso.
* Capacidad para obtener licencias y guardar recursos de Adobe Stock en AEM Assets
* Capacidad de buscar recursos similares de Adobe Stock en la interfaz de usuario de AEM Assets
* Ver un recurso seleccionado de la búsqueda de existencias en AEM Assets en el sitio web de Adobe Stock
* Los archivos de recursos con licencia están marcados con un distintivo azul con licencia para facilitar la identificación

### Metadatos de recurso

* Los recursos con licencia se almacenan en AEM Assets. Las propiedades de los recursos contienen metadatos de stock en una pestaña de metadatos de un recurso independiente
* Posibilidad de agregar referencias de licencia a metadatos de recursos

### Perfil de Asset Stock

* Un usuario puede seleccionar un perfil de Adobe Stock en *Usuario > Mis preferencias > Configuración de stock*
* Se pueden agregar referencias opcionales y obligatorias a la ventana Licencias de recursos.
* Posibilidad de elegir la preferencia de idioma para la ventana de licencias de recursos en función de la región.

### Filtro

* Un usuario puede filtrar los recursos de stock en función del tipo de recurso, la orientación y la vista similares
* El tipo de recurso incluye fotos, ilustraciones, vectores, vídeos, plantillas, 3D, Premium, editoriales
* La orientación incluye Horizontal, Vertical y Cuadrado.
* Ver un filtro similar requiere un número de archivo de Adobe Stock

### Control de acceso

* Los administradores pueden proporcionar permisos a determinados usuarios o grupos para obtener una licencia de los recursos de existencias al configurar la configuración del servicio en la nube de Adobe Stock.
* Si un usuario o grupo específico no tiene permiso para obtener una licencia de los activos de stock, se deshabilitaría la capacidad *Búsqueda de activos de Stock / Licencias de activos*.

## Configurar Adobe Stock con AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 proporciona a los usuarios la capacidad de buscar, previsualizar, guardar y licenciar recursos de Adobe Stock directamente desde AEM. En este vídeo se explica cómo configurar Adobe Stocks con AEM Assets mediante la consola de Adobe I/O.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Para la configuración del servicio de Adobe Stock Cloud, se debe seleccionar el Entorno de PROD y la ruta de acceso de recursos con licencia para que apunten a /content/dam. El campo Entorno se eliminaría en la próxima versión de AEM y la ruta de acceso de los recursos con licencia formaría parte de una función futura y la compatibilidad con este campo se introducirá en la próxima versión de AEM.

>[!NOTE]
>
>La integración requiere un [plan empresarial de Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) y AEM 6.4 con al menos [Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) implementado. Para obtener más información sobre el paquete de servicio de AEM 6.4, consulte estas [notas de la versión](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). También necesita permisos de administrador para [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) y Adobe Experience Manager para configurar la integración.

### Instalación {#installations}

* Para AEM 6.4, debe instalar el [AEM Service Pack 2](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq640/servicepack/AEM-6.4.2.0) y luego volver a instalar el archivo cq-dam-stock-integration-content-1.0.4.zip.
* Asegúrese de tener permisos de administrador en [Adobe I/O Console](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) y Adobe Experience Manager para configurar la integración.

#### Configuración de Adobe IMS mediante Adobe I/O Console {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Cree una configuración de cuenta técnica de Adobe IMS en **Herramientas > Seguridad**
2. Seleccione *Cloud Solution* como *Adobe Stock* y cree un nuevo certificado o reutilice un certificado existente para la configuración.
3. Vaya a la consola de Adobe I/O y cree una nueva integración de cuenta de servicio para *Adobe Stock*.
4. Cargue el certificado del paso 2 a su integración de cuenta de Adobe Stock Service.
5. Elija la configuración de perfil de Adobe Stock necesaria y complete la integración de servicio.
6. Utilice los detalles de integración para completar la configuración de la cuenta técnica de Adobe IMS
7. Asegúrese de que puede recibir el token de acceso mediante la cuenta técnica de Adobe IMS.

![Cuenta técnica de IMS de Adobe](assets/screen_shot_2018-10-22at12219pm.png)

#### Configurar los servicios de Adobe Stock Cloud {#set-up-adobe-stock-cloud-services}

1. Cree una nueva configuración de servicio en la nube para Adobe Stock en **Herramientas > Cloud Services.**
2. Seleccione la *Configuración de Adobe IMS* creada en la sección anterior para la configuración de *Adobe Stock Cloud*

3. Asegúrese de seleccionar **ENVIRONMENT** como PROD. El entorno de ensayo no es compatible y se eliminará en la próxima versión de AEM.
4. **La** ruta de acceso de los recursos con licencia se puede señalar a cualquier directorio bajo /content/dam. La compatibilidad con funciones de este campo se agregará en la próxima versión de AEM
5. Seleccione la configuración regional y complete la configuración.
6. También puede agregar usuarios/grupos al servicio de Adobe Stock Cloud para habilitar el acceso para usuarios o grupos específicos.

![Configuración de Adobe Assets Stock](assets/screen_shot_2018-10-22at12425pm.png)

### Recursos adicionales

* [Plan de stock empresarial](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notas de la versión de AEM 6.4 Service Pack 2](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html)
* [Integración de AEM y Adobe Stock](https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html#IntegrateAEMandAdobeStock)
* [API de integración de la consola de Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentos de API de Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)