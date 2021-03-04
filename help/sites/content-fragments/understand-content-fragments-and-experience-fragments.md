---
title: Explicación de los fragmentos de contenido y los fragmentos de experiencias
description: Los fragmentos de contenido y los fragmentos de experiencia de Adobe Experience Manager pueden parecer similares en la superficie, pero cada uno desempeña un papel clave en diferentes casos de uso. Descubra cómo los fragmentos de contenido y los fragmentos de experiencias son similares, diferentes, y cuándo y cómo se utilizan cada uno.
sub-product: recursos, sitios, servicios de contenido
feature: Fragmentos de contenido, fragmentos de experiencias
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Administración de contenido
role: Profesional empresarial
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 3%

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
<li>Una composición reutilizable de uno o más componentes de AEM que definen el contenido y la presentación que forma una <strong>experiencia</strong> que tiene sentido por sí sola</li>
</ul>
</td>
</tr><tr><td><strong>Inquilinos principales</strong></td>
<td><ul>
<li>Centrado en el contenido</li>
<li>Definido por un <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">modelo de datos estructurado, basado en formularios.</a></li>
<li>Diseño y diseño independiente.</li>
<li>El canal es propietario de la presentación del contenido del fragmento de contenido (diseño y diseño)</li>
</ul>
</td>
<td><ul>
<li>Centrado en la presentación</li>
<li>Definido por la composición no estructurada de los componentes de AEM</li>
<li>Define el diseño y el diseño del contenido</li>
<li>Se utiliza "tal cual" en los canales</li>
</ul>
</td>
</tr><tr><td><strong>Detalles técnicos</strong></td>
<td><ul>
<li>Implementado como <strong>dam:Asset</strong></li>
<li>Definido por un <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">Modelo de fragmento de contenido</a></li>
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
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Creación de </a> bloques para permitir la reutilización de contenido entre variaciones</li>
</ul>
</td>
</tr><tr><td><strong>Características</strong></td>
<td><ul>
<li>Variaciones</li>
<li>Versiones</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank"></a> Sincronización del contenido entre variaciones</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">Diferencia visual </a> de las versiones de fragmentos de contenido</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank"></a> Anotaciones de elementos de texto multilínea</li>
<li>Resumen <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">inteligente</a> de elementos de texto multilínea.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">Traducción/localización</a></li>
</ul>
</td>
<td><ul>
<li>Variaciones</li>
<li>Variaciones como Live Copies</li>
<li>Versiones</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Componentes</a></li>
<li>Anotaciones</li>
<li>Presentación y vista previa adaptables</li>
<li>Traducción/localización</li>
</ul>
</td>
</tr><tr><td><strong>Uso</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">Componentes principales de AEM </a> Componentes de fragmento de contenido para su uso en AEM Sites, AEM Screens o en fragmentos de experiencias.</li>
<li>Exportación JSON mediante <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a> para consumo de terceros</li>
<li>JSON a través de las API de AEM HTTP Assets para el consumo de terceros.</li>
</ul>
</td>
<td><ul>
<li>Componente Fragmento de experiencia de AEM para su uso en AEM Sites, AEM Screens u otros fragmentos de experiencias.</li>
<li>Exportar como <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">HTML sin formato</a> para su uso por sistemas de terceros</li>
<li><a href="https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">Exportación de HTML a Adobe </a> Target para ofertas segmentadas</li>
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
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guía del usuario de fragmentos de contenido de AEM</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">Uso de fragmentos de contenido en AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Documentación de Adobe sobre fragmentos de experiencias</a></li>
</ul>
</td>
</tr></tbody></table>

## Arquitectura de fragmentos de contenido

El diagrama siguiente ilustra la arquitectura general de los fragmentos de contenido de AEM

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

+ **Plantillas editables**, que a su vez se definen mediante  **Tipos de plantilla editables** y una implementación de componentes de página de  **AEM**, definen los componentes de AEM permitidos que se pueden utilizar para componer un fragmento de experiencia.
+ El **fragmento de experiencia** es una instancia de una plantilla editable que representa una experiencia lógica.
+ Sin embargo, las **variaciones** del fragmento de experiencia se adhieren a la plantilla editable y tienen variaciones en la experiencia (contenido y diseño).
+ Los fragmentos de experiencias pueden ser expuestos o consumidos por:
   + Uso de fragmentos de experiencia en AEM Sites (o AEM Screens) mediante el componente Fragmento de experiencia de AEM.
   + Exponer un contenido de variaciones de fragmentos de experiencias como JSON (con HTML incrustado) a través de **AEM Content Services** y páginas de API.
   + Exponiendo directamente una variación de fragmento de experiencia como **&quot;HTML sin formato&quot;**.
   + Exportación de fragmentos de experiencia a **Adobe Target** como ofertas HTML o JSON.
   + AEM Sites admite ofertas HTML de forma nativa, pero las ofertas JSON requieren un desarrollo personalizado.

## Materiales de apoyo para fragmentos de contenido

+ [Guía del usuario de fragmentos de contenido](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Uso de fragmentos de contenido en AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [Componente de fragmento de contenido de los componentes principales de WCM de AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Uso de fragmentos de contenido y servicios de contenido de AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [Introducción a los servicios de contenido de AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## Materiales de apoyo para fragmentos de experiencias

+ [Documentación de Adobe sobre fragmentos de experiencias](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [Explicación de los fragmentos de experiencia de AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [Uso de fragmentos de experiencia de AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Uso de fragmentos de experiencia de AEM con Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
