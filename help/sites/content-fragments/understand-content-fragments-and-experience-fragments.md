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
duration: 242
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
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
<li>Reutilizable, independiente de la presentación <strong>content</strong>, compuesto por elementos de datos estructurados (texto, fechas, referencias, etc.)</li>
</ul>
</td>
<td><ul>
<li>AEM Un componente compuesto reutilizable de uno o más componentes de la lista de componentes que definen el contenido y la presentación y que forma un componente de la lista de componentes que se pueden usar. <strong>experiencia</strong> que tiene sentido por sí solo</li>
</ul>
</td>
</tr><tr><td><strong>Inquilinos principales</strong></td>
<td><ul>
<li>Centrado en el contenido</li>
<li>Definido por una <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">modelo de datos estructurado, basado en formularios.</a></li>
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
<li>Definido por una <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Modelo de fragmento de contenido</a></li>
</ul>
</td>
<td><ul>
<li>Implementado como <strong>cq:Page</strong></li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Bloques de creación</a> permitir la reutilización del contenido en todas las variaciones</li>
</ul>
</td>
</tr><tr><td><strong>Características</strong></td>
<td><ul>
<li>Variaciones</li>
<li>Versiones</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Sincronización</a> de contenido entre variaciones</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Diferencia visual</a> de versiones de fragmentos de contenido</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">Anotaciones</a> de elementos de texto multilínea</li>
<li>Inteligente <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">resumen</a> de elementos de texto multilínea.</li>
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
<li>Exportación de JSON mediante <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=es">AEM API de GraphQL sin encabezado</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es" target="_blank">AEM Componente de fragmento de contenido de componentes principales</a> para su uso en AEM Sites, AEM Screens o en fragmentos de experiencias.</li>
<li>Exportación de JSON mediante <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Servicios de contenido</a> para consumo de terceros</li>
<li>Exportación de JSON a Adobe Target para ofertas segmentadas</li>
<li>AEM JSON a través de API de recursos HTTP para consumo de terceros</li>
</ul>
</td>
<td><ul>
<li>AEM Componente Fragmento de experiencia para su uso en AEM Sites, AEM Screens u otros fragmentos de experiencias.</li>
<li>Exportar como <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">HTML sin formato</a> para su uso en sistemas de terceros</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Exportación del HTML a Adobe Target</a> para ofertas segmentadas</li>
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

+ **Modelos de fragmento de contenido** defina los elementos (o campos) que definen qué contenido puede capturar y exponer el fragmento de contenido.
+ El **Fragmento de contenido** es una instancia de un modelo de fragmento de contenido que representa una entidad de contenido lógica.
+ Fragmento de contenido **variaciones** adherirse al modelo de fragmento de contenido, sin embargo, tiene variaciones en el contenido.
+ Los fragmentos de contenido pueden ser expuestos o consumidos por:
   + Uso de fragmentos de contenido en **AEM Sites** (o AEM Screens AEM) a través del componente Fragmento de contenido de los componentes principales de WCM de la manera más rápida.
   + Consumir **Fragmento de contenido** AEM desde aplicaciones sin encabezado mediante API de GraphQL sin encabezado de.
   + La exposición de un fragmento de contenido modifica el contenido como JSON mediante **AEM Servicios de contenido** y páginas API para casos de uso de solo lectura.
   + Exposición directa del contenido de fragmentos de contenido (todas las variaciones) como JSON a través de llamadas directas a AEM Assets a través de **API HTTP de AEM Assets** para casos de uso de CRUD.

## Arquitectura de fragmentos de experiencias

![Arquitectura de fragmentos de experiencias](./assets/experience-fragments-architecture.png)

+ **Plantillas editables**, que a su vez se definen mediante **Tipos de plantilla editables** y un **AEM Implementación del componente Página** AEM , defina los Componentes de la permitidos que se pueden utilizar para componer un Fragmento de experiencia.
+ El **Fragmento de experiencia** es una instancia de una plantilla editable que representa una experiencia lógica.
+ Fragmento de experiencia **variaciones** Sin embargo, adherirse a la plantilla editable tiene variaciones en la experiencia (contenido y diseño).
+ Los fragmentos de experiencias los pueden exponer o consumir:
   + Uso de fragmentos de experiencias en AEM Sites (o AEM Screens AEM) mediante el componente Fragmento de experiencia de la.
   + Exposición de contenido de variaciones de un fragmento de experiencia como JSON (con HTML incrustado) mediante **AEM Servicios de contenido** y páginas de API.
   + Exponer directamente una variación de fragmento de experiencia como **&quot;HTML sin formato&quot;**.
   + Exportación de fragmentos de experiencias a **Adobe Target** como ofertas de HTML o JSON.
   + AEM Sites admite de forma nativa las ofertas de HTML, pero las ofertas JSON requieren un desarrollo personalizado.

## Recurso de apoyo para fragmentos de contenido

+ [Guía del usuario de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Introducción a Adobe Experience Manager como CMS sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=es)
+ [AEM Uso de fragmentos de contenido en la](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM Componente de fragmento de contenido de los componentes principales de WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=es)
+ [AEM Uso de fragmentos de contenido y sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [AEM Introducción a los servicios de contenido de](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Recurso de apoyo para fragmentos de experiencias

+ [Documentación de Adobe sobre fragmentos de experiencias](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [AEM Explicación de fragmentos de experiencia](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [AEM Uso de fragmentos de experiencia](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [AEM Uso de fragmentos de experiencia con Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
