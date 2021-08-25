---
title: Explicación de los fragmentos de contenido y los fragmentos de experiencias
description: Los fragmentos de contenido y los fragmentos de experiencia de Adobe Experience Manager pueden parecer similares en la superficie, pero cada uno desempeña un papel clave en diferentes casos de uso. Descubra cómo los fragmentos de contenido y los fragmentos de experiencias son similares, diferentes, y cuándo y cómo se utilizan cada uno.
sub-product: assets, sites, content services
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1037'
ht-degree: 1%

---


# Explicación de los fragmentos de contenido y los fragmentos de experiencias

Los fragmentos de contenido y los fragmentos de experiencia de Adobe Experience Manager pueden parecer similares en la superficie, pero cada uno desempeña un papel clave en diferentes casos de uso. Descubra cómo los fragmentos de contenido y los fragmentos de experiencias son similares, diferentes, y cuándo y cómo se utilizan cada uno.

## Comparación de fragmentos de contenido y fragmentos de experiencias

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Fragmentos de contenido (CF)</strong></td>
<td><strong>Fragmentos de experiencias (XF)</strong></td>
</tr><tr><td><strong>Definición</strong></td>
<td><ul>
<li>Contenido <strong>reutilizable</strong> no basado en la presentación, compuesto por elementos de datos estructurados (texto, fechas, referencias, etc.)</li>
</ul>
</td>
<td><ul>
<li>Una composición reutilizable de uno o más componentes AEM que definen el contenido y la presentación que forma una <strong>experiencia</strong> que tiene sentido por sí sola</li>
</ul>
</td>
</tr><tr><td><strong>Inquilinos principales</strong></td>
<td><ul>
<li>Centrado en el contenido</li>
<li>Definido por un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">modelo de datos estructurado, basado en formularios.</a></li>
<li>Diseño y diseño independiente.</li>
<li>El canal es propietario de la presentación del contenido del fragmento de contenido (diseño y diseño)</li>
</ul>
</td>
<td><ul>
<li>Centrado en la presentación</li>
<li>Definido por la composición no estructurada de AEM componentes</li>
<li>Define el diseño y el diseño del contenido</li>
<li>Se utiliza "tal cual" en los canales</li>
</ul>
</td>
</tr><tr><td><strong>Detalles técnicos</strong></td>
<td><ul>
<li>Implementado como <strong>dam:Asset</strong></li>
<li>Definido por un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Modelo de fragmento de contenido</a></li>
</ul>
</td>
<td><ul>
<li>Implementado como <strong>cq:Page</strong></li>
<li>Definido por plantillas editables</li>
<li>Representación HTML nativa</li>
</ul>
</td>
</tr><tr><td><strong>Variaciones</strong></td>
<td><ul>
<li>La variación principal es la variación canónica</li>
<li>Las variaciones son específicas del caso de uso, que pueden alinearse con los canales.</li>
</ul>
</td>
<td><ul>
<li>Las variaciones son específicas del canal o del contexto</li>
<li>Las variaciones se mantienen sincronizadas mediante AEM Live Copy</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Creación de </a> bloques para permitir la reutilización de contenido entre variaciones</li>
</ul>
</td>
</tr><tr><td><strong>Características</strong></td>
<td><ul>
<li>Variaciones</li>
<li>Versiones</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank"></a> Sincronización del contenido entre variaciones</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Diferencia visual </a> de las versiones de fragmentos de contenido</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank"></a> Anotaciones de elementos de texto multilínea</li>
<li>Resumen <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">inteligente</a> de elementos de texto multilínea.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">Traducción/localización</a></li>
</ul>
</td>
<td><ul>
<li>Variaciones</li>
<li>Variaciones como Live Copies</li>
<li>Versiones</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">Componentes</a></li>
<li>Anotaciones</li>
<li>Presentación y vista previa adaptables</li>
<li>Traducción/localización</li>
</ul>
</td>
</tr><tr><td><strong>Uso</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM componente Fragmento de contenido de componentes principales </a> para su uso en AEM Sites, AEM Screens o en fragmentos de experiencias.</li>
<li>Exportación de JSON mediante <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> para consumo de terceros</li>
<li>JSON a través de AEM API de recursos HTTP para consumo de terceros.</li>
</ul>
</td>
<td><ul>
<li>AEM componente Fragmento de experiencia para su uso en AEM Sites, AEM Screens u otros fragmentos de experiencias.</li>
<li>Exportar como <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">HTML sin formato</a> para su uso por sistemas de terceros</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Exportación de HTML a Adobe </a> Target para ofertas segmentadas</li>
<li>Exportación de JSON a Adobe Target para ofertas segmentadas</li>
</ul>
</td>
</tr><tr><td><strong>Casos de uso comunes</strong></td>
<td><ul>
<li>Contenido basado en formularios/entrada de datos muy estructurado</li>
<li>Contenido editorial de forma larga (elemento de varias líneas)</li>
<li>Contenido administrado fuera del ciclo de vida de los canales que lo entregan</li>
</ul>
</td>
<td><ul>
<li>Gestión centralizada de material promocional multicanal mediante variaciones por canal.</li>
<li>El contenido se reutiliza en varias páginas de un sitio web.</li>
<li>Chrome del sitio web (por ejemplo, encabezado y pie de página)</li>
<li>Una experiencia administrada fuera del ciclo de vida de los canales que la entregan</li>
</ul>
</td>
</tr><tr><td><strong>Documentación</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guía del usuario de fragmentos de contenido de AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">Uso de fragmentos de contenido en AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Documentación de Adobe sobre fragmentos de experiencias</a></li>
</ul>
</td>
</tr></tbody></table>

## Arquitectura de fragmentos de contenido

El diagrama siguiente ilustra la arquitectura general de los fragmentos de contenido AEM

!![Arquitectura de fragmentos de contenido](./assets/content-fragments-architecture.png)

+ **Los** modelos de fragmento de contenido definen los elementos (o campos) que definen qué contenido puede capturar y exponer el fragmento de contenido.
+ El **fragmento de contenido** es una instancia de un modelo de fragmento de contenido que representa una entidad de contenido lógico.
+ Sin embargo, las **variaciones** del fragmento de contenido se adhieren al modelo de fragmento de contenido.
+ Los fragmentos de contenido se pueden exponer o consumir mediante:
   + Uso de fragmentos de contenido en **AEM Sites** (o AEM Screens) mediante el componente Fragmento de contenido de los componentes principales de WCM de AEM.
   + Incrustar un fragmento de contenido en un **fragmento de experiencia** mediante el componente Fragmento de contenido de los componentes principales de WCM de AEM para utilizarlo en cualquier caso de uso de fragmento de experiencia.
   + Exponer un contenido de variaciones de fragmento de contenido como JSON a través de **AEM Content Services** y páginas de API para casos de uso de solo lectura.
   + Explicar directamente el contenido del fragmento de contenido (todas las variaciones) como JSON mediante llamadas directas a AEM Assets a través de la **API HTTP de AEM Assets** para casos de uso de CRUD.

## Arquitectura de fragmentos de experiencias

!![Arquitectura de fragmentos de experiencias](./assets/experience-fragments-architecture.png)

+ **Plantillas editables**, que a su vez se definen mediante  **Tipos de plantilla editables y una** implementación **de componentes de página** AEM, definen los componentes AEM permitidos que se pueden utilizar para componer un fragmento de experiencia.
+ El **fragmento de experiencia** es una instancia de una plantilla editable que representa una experiencia lógica.
+ Sin embargo, las **variaciones** del fragmento de experiencia se adhieren a la plantilla editable y tienen variaciones en la experiencia (contenido y diseño).
+ Los fragmentos de experiencias pueden ser expuestos o consumidos por:
   + Uso de fragmentos de experiencia en AEM Sites (o AEM Screens) mediante el componente Fragmento de experiencia de AEM.
   + Exponer un contenido de variaciones de fragmentos de experiencias como JSON (con HTML incrustado) a través de **AEM Content Services** y páginas de API.
   + Exponiendo directamente una variación de fragmento de experiencia como **&quot;HTML sin formato&quot;**.
   + Exportación de fragmentos de experiencia a **Adobe Target** como ofertas HTML o JSON.
   + AEM Sites admite ofertas HTML de forma nativa, pero las ofertas JSON requieren un desarrollo personalizado.

## Materiales de apoyo para fragmentos de contenido

+ [Guía del usuario de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Uso de fragmentos de contenido en AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM del componente Fragmento de contenido de los componentes principales de WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Uso de fragmentos de contenido y AEM sin encabezado](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [Introducción a AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Materiales de apoyo para fragmentos de experiencias

+ [Documentación de Adobe sobre fragmentos de experiencias](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Explicación de AEM fragmentos de experiencias](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Uso de AEM fragmentos de experiencias](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Uso de AEM fragmentos de experiencias con Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
