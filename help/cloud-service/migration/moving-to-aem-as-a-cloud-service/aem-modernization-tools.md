---
title: Uso de AEM herramientas de modernización para pasar a AEM as a Cloud Service
description: Descubra cómo AEM Herramientas de modernización se utilizan para actualizar un proyecto y contenido de AEM existente para que sean compatibles con AEM as a Cloud Service.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 1%

---


# Herramientas de modernización de AEM

Descubra cómo AEM Herramientas de modernización se utilizan para actualizar un contenido existente de AEM Sites para que sea compatible con AEM as a Cloud Service y se ajuste a las prácticas recomendadas.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## Uso de AEM herramientas de modernización

![Ciclo de vida de las herramientas de modernización AEM](./assets/aem-modernization-tools.png)

AEM herramientas de modernización convierten automáticamente las páginas de AEM existentes compuestas de plantillas estáticas heredadas, componentes de base y parsys para utilizar enfoques modernos, como plantillas editables, AEM componentes WCM principales y contenedores de diseño.

### Actividades clave

+ Clone la producción de AEM 6.x para ejecutar AEM herramientas de modernización
+ Descargue e instale el [herramientas de modernización de AEM más recientes](https://github.com/adobe/aem-modernize-tools/releases/latest) sobre el clon de producción de AEM 6.x mediante el Administrador de paquetes

+ [Conversor de estructura de página](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) actualiza el contenido de una página existente de una plantilla estática a una plantilla editable asignada mediante contenedores de diseño
   + Definir reglas de conversión mediante la configuración OSGi
   + Ejecutar el conversor de estructura de página con páginas existentes

+ [Conversor de componentes](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) actualiza el contenido de una página existente de una plantilla estática a una plantilla editable asignada mediante contenedores de diseño
   + Definir reglas de conversión mediante definiciones de nodos JCR/XML
   + Ejecutar la herramienta Conversor de componentes en páginas existentes

+ [Importador de políticas](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) crea directivas a partir de la configuración de diseño
   + Definir reglas de conversión utilizando definiciones de nodo JCR/XML
   + Ejecutar el Importador de directivas con definiciones de diseño existentes
   + Aplicar políticas importadas a componentes y contenedores de AEM

+ [Conversor de diálogos](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) convierte los cuadros de diálogo de componentes basados en Classic(ExtJS) y CoralUI 2 en cuadros de diálogo basados en CoralUI 3 TouchUI.
   + Ejecute la herramienta Conversor de diálogos con los cuadros de diálogo existentes basados en la interfaz de usuario de ExtJS o Coral2
   + Sincronizar los cuadros de diálogo convertidos de nuevo en el repositorio de Git

### Otros recursos

+ [Descargar AEM herramientas de modernización](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentación de las herramientas de modernización de AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems: presentación de AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
