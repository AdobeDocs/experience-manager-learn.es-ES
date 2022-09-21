---
title: Jerarquía del sitio, taxonomía y consejos de etiquetado
description: Jerarquía del sitio, taxonomía y consejos sobre etiquetado Prácticas recomendadas
hide: true
hidefromtoc: true
source-git-commit: 3eb429039589ae26a81bc6d24f020a77517133e8
workflow-type: tm+mt
source-wordcount: '2053'
ht-degree: 0%

---


# Etiquetas, taxonomía y prácticas recomendadas de metadatos: Resumen de alto nivel

Los metadatos y las etiquetas son fundamentales para mejorar la eficacia de las AEM. Los usuarios, los líderes y la administración se dan cuenta de la necesidad de una estrategia integral, pero les resulta difícil avanzar. A menudo, el conocimiento se mantiene aislado entre los usuarios, lo que dificulta la estrategia holística, y hace que los ajustes sean aún más problemáticos.

¿Cuál es la diferencia entre metadatos y etiquetas? ¿Cuáles son los aspectos empresariales que hay que tener en cuenta al impulsar su estrategia?

Se puede encontrar un resumen más intensivo [here](https://adobe.sharepoint.com/:w:/r/sites/ACSSuccessServices/_layouts/15/Doc.aspx?sourcedoc=%7BFE5E873A-A3B6-4F40-BF22-A2C9F1269802%7D&amp;file=AEM_TagTaxonomyAndMetadata_BestPractice_en%20(2).docx&amp;action=default&amp;mobileredirect=true).

## ¿Cuál es el propósito de los metadatos?

Los metadatos agregan estructura a contenido menos estructurado.
Ejemplo: Una imagen básica tiene píxeles. Podemos llamarlos &quot;datos principales&quot;. Son los metadatos los que describen el formato, la categoría, los detalles de las licencias, etc.
Los metadatos se utilizan con mayor frecuencia para los recursos. Sin embargo, también hay un gran número de casos de uso para metadatos en páginas de contenido o fragmentos de experiencia.

## Fuentes de metadatos

Las siguientes son las categorías en las que se pueden generar metadatos:

* Metadatos extraídos : la información ya está disponible en el documento, por ejemplo, en lenguaje natural.
* Metadatos derivados : la información no está disponible en los datos originales, pero se puede derivar haciendo referencia a conocimientos anteriores.
* Metadatos añadidos manualmente : son metadatos que no entran en ninguna de las primeras categorías y que un usuario debe agregar manualmente.

## Tipos de metadatos

Dentro de las categorías enumeradas arriba hay cuatro tipos principales:

* Metadatos técnicos y descriptivos: Proporciona información sobre detalles técnicos del contenido (p. ej., hora, idioma, etc.)
* Metadatos operativos: Documenta el ciclo de vida de un recurso (es decir, aprobado, en creative, campaign)
* Metadatos administrativos: El estado o el estado de un recurso dentro de una organización (es decir, información de licencia, propiedad)
* Metadatos estructurales: Ayuda a categorizar recursos o páginas para facilitar el proceso empresarial (se aplica a la mayoría de las etiquetas y taxonomías)

## Nombres de carpeta y archivo

Las carpetas son una forma natural de navegar y examinar el contenido en AEM. ¿Cómo interactuarán sus partes interesadas con AEM? Esto determinará cómo están estructuradas las carpetas. Normalmente, la estructura de carpetas se diseñó teniendo en cuenta una (o dos) de las siguientes opciones:

* Navegación
* Exploración
* Categorización
* Control de acceso

Para AEM Sites, la navegación es clave. Las carpetas se utilizan para controlar el acceso a los recursos y las páginas.

¿Qué niveles de autores van a necesitar acceso a las páginas de inicio? ¿Qué sucede con las páginas de producto? ¿O campaña? Utilice permisos más estructura de carpetas para colocar el control adecuado.

## Almacenamiento de metadatos

Existen tres formas de almacenar metadatos:

* Binario: El formato binario está relacionado con la naturaleza del recurso (Photoshop, InDesign, PNG, JPG).
* Nodo de recursos: Son metadatos del propio recurso, independientemente del sistema o proceso que se utilice.
* Ubicación externa: Metadatos que no están directamente en el recurso, pero que pueden utilizarse como descriptor del &quot;estado&quot; de un recurso (por ejemplo: un flujo de trabajo que puede afectar a un recurso pero que no se le aplica directamente)

## Modelo de metadatos

La estructura de cómo se capturan y formatean los metadatos se denomina modelo de metadatos o esquema de metadatos. Esto debe acordarse antes de que los recursos o las páginas se incorporen al sistema.

Normalmente, un modelo de metadatos se diseña para que cumpla los siguientes casos de uso:

* Buscar y recuperar: Ayuda a almacenar aspectos clave del contenido que ayudan a la fácil recuperación por parte del negocio.
* Reutilizar: Ayuda a utilizar activos antiguos para su reutilización (ahorra tiempo y dinero)
* Administración de licencias: Rastrea la propiedad de los activos por parte de la organización (a menudo por motivos legales)
* Distribuir: Poner el contenido a disposición de los consumidores o distribuir los recursos entre socios comerciales.
* Archivo: Los metadatos que anotan un recurso están obsoletos (siempre se recomienda colocar un indicador &quot;archivado&quot; en el recurso para no perder información vital)/
* Referencia cruzada: Metadatos asociados que capturan la relación entre dos o más activos (la síntesis de metadatos permite las referencias cruzadas y la organización de grupos coherente)
* Navegar: Estructura de carpetas en la que se almacenan los recursos (se utiliza para recuperar información explorando)

*Los metadatos de autor son compatibles principalmente con los procesos operativos. La publicación admite casos de uso de recuperación y distribución.*

## Uso de etiquetas como términos predefinidos

Una etiqueta es una palabra clave o término asignado a un fragmento de información. Por ejemplo, en lugar de introducir &quot;coche&quot;, &quot;vehículo&quot;, &quot;automóvil&quot;, un sistema de etiquetas permite elegir un solo valor, lo que hace que la búsqueda sea más predecible.  Las etiquetas normalizan y simplifican la categorización de los recursos.

*Nota: Aunque AEM permite el etiquetado ad-hoc, es recomendable mantenerse alejado de esto, ya que podría provocar una taxonomía indefinida y difícil de manejar.*

Uso común de las etiquetas:

* Búsqueda de palabras clave: Una etiqueta puede describir que un recurso pertenece a un grupo determinado de entidades. Por ejemplo, una etiqueta &quot;image/subject/car&quot; describe el recurso pertenece al conjunto de imágenes que muestran un coche.
* Conducción de relaciones: Todos los recursos que comparten la misma etiqueta pueden considerarse conectados. Etiquetar en lugar de vincular directamente es especialmente útil en los sitios web que tienen mucho contenido dinámico y conectado.
* Navegación de la unidad: Las etiquetas ordenadas en taxonomía jerárquica pueden generar navegación, un vínculo o documentos similares.
Las etiquetas también deben verse como información que conecta varios tipos de datos en función de términos comerciales en lugar de propiedades técnicas.

## Aplicaciones comunes de etiquetas

Cuando se utilizan en AEM, las etiquetas pueden ayudar a lograr una implementación mucho más corta de la función compleja, como:

* Búsqueda con facetas
* Navegación personalizada
* Contenido relacionado
* Referencias de contenido
* Optimización del motor de búsqueda
* Resaltado de conceptos clave

## Taxonomías

Una taxonomía es un sistema de organización de etiquetas basado en características compartidas, que generalmente están estructuradas jerárquicamente según las necesidades de la organización. La estructura puede ayudar a encontrar una etiqueta más rápido o imponer una generalización.
Ejemplo: Es necesario subclasificar las imágenes de los automóviles.  La taxonomía podría tener el siguiente aspecto:

/subject/car/ /subject/car/sportscar /subject/car/sportscar/porsche /subject/car/sportscar/ferrari ... /subject/car/minivan /subject/car/minivan/mercedes /subject/car/minivan/volkswagen ... /subject/car/limousine ...

Ahora un usuario puede elegir si quiere buscar imágenes de los bufones deportivos en general o un &quot;Porsche&quot; en particular. Después de todo, ambos son cicatrices deportivas.
Práctica recomendada: Evite taxonomías planas. Las taxonomías planas carecen de los beneficios descritos anteriormente y requieren un mantenimiento constante

**Uso de una taxonomía como Tesauro.**  Cuando un usuario busca una palabra clave, el sistema crea una segunda búsqueda de todos los sinónimos encontrados allí.
Además, en lugar de escribir &quot;coche&quot; manualmente, el sistema puede proporcionar una lista de palabras clave para mejorar la coherencia.

**Uso de una taxonomía como diccionario.** En lugar de imprimir &quot;coche&quot;, puede expandir la etiqueta única y utilizar todos los sinónimos de la etiqueta.

**Varias categorías.** A diferencia de la jerarquía de carpetas, las etiquetas se pueden utilizar para expresar varias categorías al mismo tiempo. Un recurso etiquetado con:

/subject/car/minivan/mercedes /subject/people/family /color/red

## Metadatos frente a etiquetas

No todos los metadatos deben considerarse como candidatos para el sistema de etiquetado. Los metadatos técnicos pueden duplicar innecesariamente la información. El mejor candidato para las etiquetas son los metadatos empresariales. Las etiquetas .son una buena opción para aplicar un vocabulario coherente, una búsqueda con facetas y la navegación.

## Administración de etiquetas

La administración de etiquetas cuenta con un equipo principal dedicado. Los nuevos miembros deben conocer primero el propósito y la función de la taxonomía antes de agregar nuevas etiquetas.  Los expertos temporizados, que actúan como guardianes de las nuevas etiquetas, reducirán la incoherencia a largo plazo.

## Creación de etiquetas

Las taxonomías deben ser utilizadas por los autores de contenido y entendidas por los usuarios finales. Deben crearse antes del proceso de creación de contenido. Cualquier método abreviado resultará en un esfuerzo adicional de administración y mantenimiento.

## Mantenimiento en curso

Las cosas cambian, y las necesidades de la lista de etiquetas también. Lleve a cabo un proceso de mantenimiento sólido que reducirá la duplicación.

Asegúrese de que los colaboradores de contenido sepan cómo pueden proponer cambios y que los editores o administradores de contenido revisen los términos regularmente.

## Prácticas recomendadas con etiquetas y taxonomías

**Estandarización de etiquetas.** Cree un glosario que proporcione un vocabulario autorizado. Sin establecer normas, la duplicación planteará problemas. Además, se recomienda auditar no solo la taxonomía sino también el uso de las etiquetas.

**No sobreetiquetar.** Las etiquetas pueden perder su significado si se distribuyen con demasiada frecuencia.Pegue etiquetas superfluas para lograr una eficacia óptima.

**Volver a evaluar las etiquetas a lo largo del tiempo.** Recuerde que la terminología comercial y el contexto empresarial rara vez permanecen estáticos. Es posible que necesite volver a estandarizar y volver a aplicar las etiquetas.

**Uso del etiquetado inteligente con tecnología de IA.** Etiquetado inteligente [consulte vínculo] es una capacidad de IA en AEM para reducir el esfuerzo de etiquetado manual de recursos. El etiquetado inteligente utiliza una IA para deducir información sobre el asunto de una imagen. Genera etiquetas descriptivas que describen el contenido de una imagen.

## Calidad y mantenimiento de metadatos

Comprender los requisitos comerciales es un paso importante en la ejecución de un modelo de administración de metadatos. Sin definición, la información no se puede almacenar. Será necesario revisar el modelo periódicamente. Esto es una actividad de control de calidad vital.

Además, los metadatos deben capturarse lo antes posible en el proceso de creación de contenido. Si los metadatos no se &quot;aplican en el momento adecuado, hay pocas posibilidades de aplicarlos de forma retroactiva.

**Utilizar metadatos** para mejorar la colaboración: Utilice Adobe Asset Link, Adobe Bridge y AEM Desktop para unir el proceso creativo y utilizar los metadatos para optimizar los flujos de trabajo creativos. El uso de estas herramientas enriquecerá los metadatos y la experiencia del usuario en todo el proceso creativo.

## Prácticas recomendadas para la administración de metadatos

* Asignar equipo principal con un mandato ejecutivo sólido: Formar un equipo básico de metadatos que tenga una comprensión completa del ecosistema empresarial y un fuerte mandato de la administración de la organización.
* Defina la estrategia y la gobernanza de los metadatos: Una buena estrategia de metadatos puede ayudar a las organizaciones a explicar la necesidad y los beneficios de los metadatos.  Una estrategia consiste en: esquemas de metadatos, taxonomía, procesos empresariales (para la calidad y captura de los datos), funciones y responsabilidades, y procesos de gobernanza. *
* Definir y comunicar el modelo de metadatos coherente: La estrategia y el razonamiento definidos deben estar bien documentados y comunicarse dentro de las organizaciones.
* Convención de nomenclatura estándar: Cree una convención de nomenclatura de archivos coherente y descriptiva para mejorar la marca, la administración de la información y la facilidad de uso.
* Caracteres seguros en los nombres de archivo: El nombre de archivo debe ser interpretado por todos los sistemas operativos comunes. Es seguro utilizar caracteres, números, diéresis, espacio y subrayado. El símbolo menos también es seguro, pero si corta y pega, puede parecer un &quot;guión&quot;.
* Convención de nomenclatura de versiones: AEM ofrece algunas funciones para mantener las versiones anteriores de los recursos. En algunos casos, es posible que desee conservar varias versiones. Sin embargo, debe asegurarse de que el esquema de versiones sea coherente.

## Metadatos organizativos y descriptivos

Algunas directrices pueden ayudarle a decidir cómo categorizar los metadatos:

**Descripción** - Si los datos describen el recurso o parte del contenido, deben formar parte de los metadatos adjuntos.

**Buscar** - Si los metadatos deben utilizarse en la búsqueda, deben adjuntarse.

**Exposición** - Si está exponiendo los metadatos en una plataforma de distribución a un tercero, asegúrese de no exponer también los metadatos &quot;internos&quot;.

**Duración** - Cuanto más tiempo se supone que viven los metadatos, más probable es que sean buenos candidatos para los metadatos adjuntos.

**Procesos comerciales relacionados** - Definitivamente es útil tener un ID de producto permanente como parte de los metadatos. Pero la categoría de un artículo en relación con el catálogo de productos es un metadato cuestionable para el recurso.

**Organización y procesamiento** - Si la naturaleza de los metadatos es de naturaleza organizativa, como el estado en un flujo de trabajo de aprobación o la propiedad de un determinado departamento, los metadatos externos deben considerarse en lugar de adjuntar los metadatos al recurso.

*Para crear la estrategia, haga las siguientes preguntas:*

* ¿Qué tipo de contenido e &quot;información adicional&quot; (= metadatos) se necesita para resolver problemas comerciales/preguntas comerciales/problemas comerciales?
* ¿Cuáles son las variables, los &quot;campos&quot; del esquema y cuáles son los valores posibles? Qué variables necesitan una entrada de texto libre, cuáles se pueden reducir por tipo (número, fecha, booleano, ...), un conjunto de valores fijos (por ejemplo, países) o etiquetas de una taxonomía determinada. ¿Cuántas etiquetas se requieren, se permiten?
* ¿Qué problemas técnicos, problemas o preguntas se pueden resolver mediante metadatos?
* ¿Cómo puede obtener/crear ese contenido/metadatos? ¿Cuánto costará obtener o crear esos metadatos?
* ¿Qué tipos de metadatos se necesitan para un grupo de usuarios determinado?
* ¿Cómo se mantendrán y actualizarán los metadatos?
* ¿Quién es el responsable de qué parte del proceso?
* ¿Cómo puede asegurar que se siguen los procesos comerciales acordados?
* ¿Qué normas debe seguir? Si adopta y modifica una norma del sector (Dublin Core, ISO 19115, PRISM, etc.) ¿O debería la organización crear sus propios estándares?
* ¿Dónde está documentada la estrategia? ¿Cómo puede asegurarse de que todas las partes interesadas tengan acceso? ¿Cómo puede asegurarse de que el personal recién incorporado cumpla la norma acordada (por ejemplo, visitar la formación antes de obtener acceso)?
