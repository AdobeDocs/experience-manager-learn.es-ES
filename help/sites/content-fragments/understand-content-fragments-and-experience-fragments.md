---
title: Fragmentos de contenido y fragmentos de experiencias
description: Conozca las similitudes y diferencias entre los fragmentos de contenido y los fragmentos de experiencias, y cuándo y cómo utilizar cada tipo.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
duration: 168
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 2%

---

# Fragmentos de contenido y fragmentos de experiencias

Los fragmentos de contenido y los fragmentos de experiencias de Adobe Experience Manager pueden parecer similares en la superficie, pero cada uno desempeña un papel clave en diferentes casos de uso. Descubra cómo los fragmentos de contenido y los fragmentos de experiencias son similares, diferentes y cuándo y cómo utilizarlos.

## Comparación

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Fragmentos de contenido (CF)</strong></td>
<td><strong>Fragmentos de experiencias (XF)</strong></td>
</tr><tr><td><strong>Definición</strong></td>
<td><ul>
<li><strong>contenido</strong> reutilizable y independiente de la presentación, compuesto por elementos de datos estructurados (texto, fechas, referencias, etc.)</li>
</ul>
</td>
<td><ul>
<li>AEM Un componente reutilizable y compuesto de uno o más componentes de la interfaz de usuario que definen contenido y presentación y que forman una <strong>experiencia</strong> que tiene sentido por sí sola.</li>
</ul>
</td>
</tr><tr><td><strong>Principios básicos</strong></td>
<td><ul>
<li>Centrado en el contenido</li>
<li>Definido por un modelo de datos <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">estructurado y basado en formularios.</a></li>
<li>Diseño independiente del diseño.</li>
<li>El canal es propietario de la presentación del contenido del fragmento de contenido (diseño)</li>
</ul>
</td>
<td><ul>
<li>Centrado en la presentación</li>
<li>AEM Definido por la composición no estructurada de componentes de la</li>
<li>Define el diseño del contenido</li>
<li>Se utiliza "tal cual" en los canales</li>
</ul>
</td>
</tr><tr><td><strong>Detalles técnicos</strong></td>
<td><ul>
<li>Implementado como <strong>dam:Asset</strong></li>
<li>Definido por un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">modelo de fragmento de contenido</a></li>
</ul>
</td>
<td><ul>
<li>Se implementó como <strong>cq:Page</strong></li>
<li>Definido por plantillas editables</li>
<li>Representación del HTML nativo</li>
</ul>
</td>
</tr><tr><td><strong>Variaciones</strong></td>
<td><ul>
<li>La variación Principal es la variación canónica</li>
<li>Las variaciones son específicas de casos de uso, que pueden alinearse con los canales.</li>
</ul>
</td>
<td><ul>
<li>Las variaciones son específicas del canal o del contexto</li>
<li>AEM Las variaciones se mantienen sincronizadas mediante Live Copy</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Bloques de creación</a> permiten la reutilización de contenido entre variaciones</li>
</ul>
</td>
</tr><tr><td><strong>Características</strong></td>
<td><ul>
<li>Variaciones</li>
<li>Versiones</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Sincronización</a> de contenido entre variaciones</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Diferencia visual</a> de las versiones de fragmentos de contenido</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">Anotaciones</a> de elementos de texto multilínea</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">resumen</a> inteligente de elementos de texto multilínea.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">Traducción/localización</a></li>
</ul>
</td>
<td><ul>
<li>Variaciones</li>
<li>Variaciones como Live Copies</li>
<li>Versiones</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">Componentes</a></li>
<li>Anotaciones</li>
<li>Diseño interactivo y vista previa</li>
<li>Traducción/localización</li>
<li>Modelo de datos complejo mediante referencias de fragmentos de contenido</li>
<li>Vista previa en la aplicación</li>
</ul>
</td>
</tr><tr><td><strong>Uso</strong></td>
<td><ul>
<li>AEM Exportación de JSON a través de <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=es">API de GraphQL sin encabezado</a></li>
<li>AEM <a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es" target="_blank">Componente de fragmento de contenido de componentes principales de</a> para su uso en AEM Sites, AEM Screens o en fragmentos de experiencias.</li>
<li>AEM Exportación de JSON a través de <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">Servicios de contenido </a> para el consumo de terceros</li>
<li>Exportación de JSON a Adobe Target para ofertas segmentadas</li>
<li>AEM JSON a través de API de Assets HTTP para consumo de terceros</li>
</ul>
</td>
<td><ul>
<li>AEM Componente Fragmento de experiencia para su uso en AEM Sites, AEM Screens u otros fragmentos de experiencias.</li>
<li>Exportar como <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">HTML sin formato</a> para su uso en sistemas de terceros</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Exportación del HTML a Adobe Target</a> para ofertas de destino</li>
<li>Exportación de JSON a Adobe Target para ofertas segmentadas</li>
</ul>
</td>
</tr><tr><td><strong>Casos de uso comunes</strong></td>
<td><ul>
<li>Activación de casos de uso sin encabezado mediante GraphQL</li>
<li>Entrada de datos estructurada/contenido basado en formularios</li>
<li>Contenido editorial de formato largo (elemento multilínea)</li>
<li>Contenido gestionado fuera del ciclo de vida de los canales que lo envían</li>
</ul>
</td>
<td><ul>
<li>Gestión centralizada de material promocional multicanal mediante variaciones por canal.</li>
<li>Contenido reutilizado en varias páginas de un sitio web.</li>
<li>Sitio web chrome (p. ej. header y footer)</li>
<li>Una experiencia gestionada fuera del ciclo de vida de los canales que la envían</li>
</ul>
</td>
</tr><tr><td><strong>Documentación</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM Guía del usuario de fragmentos de contenido</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">AEM Uso de fragmentos de contenido en la</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Documentación de Adobe sobre fragmentos de experiencias</a></li>
</ul>
</td>
</tr></tbody></table>

## Arquitectura de fragmentos de contenido

AEM El diagrama siguiente ilustra la arquitectura general de los fragmentos de contenido de la

![Arquitectura de fragmentos de contenido](./assets/content-fragments-architecture.png)

+ **Modelos de fragmentos de contenido** definen los elementos (o campos) que definen el contenido que el fragmento de contenido puede capturar y exponer.
+ El **fragmento de contenido** es una instancia de un modelo de fragmento de contenido que representa una entidad de contenido lógica.
+ Los fragmentos de contenido **variaciones** se adhieren al modelo de fragmento de contenido, pero tienen variaciones de contenido.
+ Los fragmentos de contenido pueden ser expuestos o consumidos por:
   + Uso de fragmentos de contenido en **AEM Sites** (o AEM Screens AEM) mediante el componente de fragmento de contenido de los componentes principales de WCM de la.
   + AEM Consumir **fragmento de contenido** de aplicaciones sin encabezado mediante las API de GraphQL sin encabezado de la interfaz de usuario de.
   + AEM Exponer contenido de variaciones de un fragmento de contenido como JSON a través de **Servicios de contenido** y páginas de API para casos de uso de solo lectura.
   + Exposición directa del contenido de fragmentos de contenido (todas las variaciones) como JSON a través de llamadas directas a AEM Assets a través de la **API HTTP de AEM Assets** para casos de uso de CRUD.

## Arquitectura de fragmentos de experiencias

![Arquitectura de fragmentos de experiencias](./assets/experience-fragments-architecture.png)

+ AEM AEM **Las plantillas editables**, que a su vez están definidas por **Tipos de plantillas editables** y una **implementación de componentes de página**, definen los componentes de permitidos que se pueden utilizar para componer un fragmento de experiencia.
+ El **Fragmento de experiencia** es una instancia de una plantilla editable que representa una experiencia lógica.
+ Las **variaciones** del fragmento de experiencia se adhieren a la plantilla editable; sin embargo, tienen variaciones en la experiencia (contenido y diseño).
+ Los fragmentos de experiencias los pueden exponer o consumir:
   + Uso de fragmentos de experiencias en AEM Sites (o AEM Screens AEM) mediante el componente Fragmento de experiencia de la.
   + Exponer contenido de variaciones de un fragmento de experiencia como JSON (con HTML AEM incrustado) a través de **Servicios de contenido** y páginas de la API de .
   + Exponiendo directamente una variación del fragmento de experiencia como **&quot;HTML sin formato&quot;**.
   + Exportando fragmentos de experiencias a **Adobe Target** como ofertas de HTML o JSON.
   + AEM Sites admite de forma nativa las ofertas de HTML, pero las ofertas JSON requieren un desarrollo personalizado.

## Recurso de apoyo para fragmentos de contenido

+ [Guía del usuario sobre fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Introducción a Adobe Experience Manager como CMS sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=es)
+ AEM [Uso de fragmentos de contenido en el elemento de contenido](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ AEM [Componente de fragmento de contenido de los componentes principales de WCM de](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es)
+ AEM [Uso de fragmentos de contenido y sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ AEM [Introducción a los servicios de contenido de](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Recurso de apoyo para fragmentos de experiencias

+ [Documentación de Adobe sobre fragmentos de experiencias](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ AEM [Explicación de los fragmentos de experiencia de la](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ AEM [Uso de fragmentos de experiencia de la aplicación](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ AEM [Uso de fragmentos de experiencia de la con Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
