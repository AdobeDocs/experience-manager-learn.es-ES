---
title: Comprender los fragmentos de contenido y los fragmentos de experiencia
description: Los fragmentos de contenido y los fragmentos de experiencia de Adobe Experience Manager pueden parecer similares en la superficie, pero cada uno de ellos desempeña funciones clave en distintos casos de uso. Conozca cómo los fragmentos de contenido y los fragmentos de experiencia son similares, diferentes y cuándo y cómo se utilizan cada uno.
sub-product: recursos, sitios, servicios de contenido
feature: content fragments, experience fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 3%

---


# Comprender los fragmentos de contenido y los fragmentos de experiencia

Los fragmentos de contenido y los fragmentos de experiencia de Adobe Experience Manager pueden parecer similares en la superficie, pero cada uno de ellos desempeña funciones clave en distintos casos de uso. Conozca cómo los fragmentos de contenido y los fragmentos de experiencia son similares, diferentes y cuándo y cómo se utilizan cada uno.

## Comparación de fragmentos de contenido y fragmentos de experiencia

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Fragmentos de contenido (CF)</strong></td>
<td><strong>Fragmentos de experiencia (XF)</strong></td>
</tr><tr><td><strong>Definición</strong></td>
<td><ul>
<li>Contenido <strong>reutilizable y no modificable</strong>, compuesto por elementos de datos estructurados (texto, fechas, referencias, etc.)</li>
</ul>
</td>
<td><ul>
<li>Composición reutilizable de uno o más componentes AEM que definen el contenido y la presentación que forman una <strong>experiencia</strong> que tiene sentido por sí sola</li>
</ul>
</td>
</tr><tr><td><strong>Inquilinos principales</strong></td>
<td><ul>
<li>Centrado en el contenido</li>
<li>Definido por un modelo de datos <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">estructurado, basado en formularios.</a></li>
<li>Diseño y maquetación no agnóstico.</li>
<li>El canal es propietario de la presentación del contenido del fragmento de contenido (diseño y maquetación)</li>
</ul>
</td>
<td><ul>
<li>Centrado en la presentación</li>
<li>Definido por la composición no estructurada de AEM componentes</li>
<li>Define el diseño y el diseño del contenido</li>
<li>Se utiliza "tal cual" en canales</li>
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
<li>La variación Master es la variación canónica</li>
<li>Las variaciones son específicas del caso de uso, lo que puede alinearse con los canales.</li>
</ul>
</td>
<td><ul>
<li>Las variaciones son específicas del canal o del contexto</li>
<li>Las variaciones se mantienen sincronizadas mediante AEM Live Copy</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Creación de </a> bloqueos para permitir la reutilización del contenido entre variaciones</li>
</ul>
</td>
</tr><tr><td><strong>Características</strong></td>
<td><ul>
<li>Variaciones</li>
<li>Versiones</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank"></a> Sincronización de contenido entre variaciones</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">Diferencia visual </a> de las versiones de fragmento de contenido</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank"></a> Anotaciones de elementos de texto de varias líneas</li>
<li>Resumen inteligente <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank"></a> de elementos de texto multilínea.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">Traducción/localización</a></li>
</ul>
</td>
<td><ul>
<li>Variaciones</li>
<li>Variaciones como Live Copies</li>
<li>Versiones</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Componentes</a></li>
<li>Anotaciones</li>
<li>Diseño y previsualización adaptables</li>
<li>Traducción/localización</li>
</ul>
</td>
</tr><tr><td><strong>Uso</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM componentes principales </a> del fragmento de contenido para su uso en AEM Sites, AEM Screens o en fragmentos de experiencia.</li>
<li>Exportación de JSON mediante <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">Servicios de contenido AEM</a> para consumo de terceros</li>
<li>JSON a través de API de recursos HTTP AEM para consumo de terceros.</li>
</ul>
</td>
<td><ul>
<li>AEM componente Fragmento de experiencia para su uso en AEM Sites, AEM Screens u otros fragmentos de experiencia.</li>
<li>Exportar como <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">HTML sin formato</a> para su uso por sistemas de terceros</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">Exportación de HTML a Adobe </a> Target para ofertas de destino</li>
<li>Exportación de JSON a Adobe Target para ofertas de objetivo</li>
</ul>
</td>
</tr><tr><td><strong>Casos de uso común</strong></td>
<td><ul>
<li>Contenido basado en formularios/entrada de datos muy estructurado</li>
<li>Contenido editorial de formato largo (elemento multilínea)</li>
<li>Contenido administrado fuera del ciclo de vida de los canales que lo entregan</li>
</ul>
</td>
<td><ul>
<li>Gestión centralizada de garantías promocionales de varios canales mediante variaciones por canal.</li>
<li>Contenido reutilizado en varias páginas de un sitio Web.</li>
<li>Chrome del sitio web (p. ej. encabezado y pie de página)</li>
<li>Una experiencia administrada fuera del ciclo de vida de los canales que la ofrecen</li>
</ul>
</td>
</tr><tr><td><strong>Documentación</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guía del usuario de fragmentos de contenido AEM</a></li>
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

+ **Los** modelos de fragmento de contenido definen los elementos (o campos) que definen el contenido que el fragmento de contenido puede capturar y exponer.
+ El **fragmento de contenido** es una instancia de un modelo de fragmento de contenido que representa una entidad de contenido lógico.
+ Sin embargo, las variaciones de fragmento de contenido **** se adhieren al modelo de fragmento de contenido y tienen variaciones de contenido.
+ Los fragmentos de contenido pueden ser expuestos o consumidos por:
   + Uso de fragmentos de contenido en **AEM Sites** (o AEM Screens) mediante el componente Fragmento de contenido de componentes principales de WCM de AEM.
   + Incrustación de un fragmento de contenido en un **fragmento de experiencia** mediante el componente Fragmento de contenido de componentes principales de WCM de AEM, para su uso en cualquier caso de uso de fragmento de experiencia.
   + La exposición de un fragmento de contenido modifica el contenido como JSON mediante **Servicios de contenido de AEM** y Páginas de API para casos de uso de solo lectura.
   + Exhibir directamente el contenido del fragmento de contenido (todas las variaciones) como JSON mediante llamadas directas a AEM Assets mediante la **API HTTP de AEM Assets** para casos de uso de CRUD.

## Arquitectura de fragmentos de experiencia

!![Arquitectura de fragmentos de experiencia](./assets/experience-fragments-architecture.png)

+ **Las plantillas** editables, que a su vez están definidas por los  **tipos de plantilla editables y una implementación**  de componente de página  **** AEM, definen los componentes AEM permitidos que se pueden utilizar para componer un fragmento de experiencia.
+ El **fragmento de experiencias** es una instancia de una plantilla editable que representa una experiencia lógica.
+ Sin embargo, el fragmento de experiencias **variaciones** se adhieren a la plantilla editable. Sin embargo, tienen variaciones en la experiencia (contenido y diseño).
+ Los fragmentos de experiencia se pueden exponer o consumir mediante:
   + Uso de fragmentos de experiencia en AEM Sites (o AEM Screens) mediante el componente Fragmento de experiencia de AEM.
   + Exponer un contenido de variaciones de fragmento de experiencia como JSON (con HTML incrustado) mediante **Servicios de contenido de AEM** y Páginas de API.
   + Exponiendo directamente una variación de fragmento de experiencia como **&quot;HTML sin formato&quot;**.
   + Exportación de fragmentos de experiencia a **Adobe Target** como ofertas HTML o JSON.
   + AEM Sites admite ofertas HTML de forma nativa, pero las ofertas JSON requieren un desarrollo personalizado.

## Materiales de apoyo para fragmentos de contenido

+ [Guía del usuario de fragmentos de contenido](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Uso de fragmentos de contenido en AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [Componente de fragmento de contenido de componentes principales de WCM AEM](https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Uso de fragmentos de contenido y servicios de contenido AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [Introducción a Servicios de contenido de AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## Materiales de apoyo para fragmentos de experiencias

+ [Documentación de Adobe sobre fragmentos de experiencias](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [Explicación de los fragmentos de experiencia AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [Uso de fragmentos de experiencias AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Uso de fragmentos de experiencia AEM con Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
