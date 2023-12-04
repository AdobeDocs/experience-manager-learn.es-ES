---
title: Guía de jerarquía de sitios, taxonomía y etiquetado
description: Información general completa sobre metadatos, etiquetado, taxonomía y jerarquía de AEM Sites. Utilice esta guía para asegurarse de que la estrategia de contenido es coherente y sigue las prácticas recomendadas
topic: Content Management
feature: Learn From Your Peers
role: Admin, User
jira: KT-14254
level: Beginner, Intermediate
doc-type: Article
exl-id: c88c3ec7-9060-43e2-a6a2-d47bba6f7cf3
duration: 549
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2035'
ht-degree: 0%

---

# Prácticas recomendadas de etiquetas, taxonomía y metadatos: resumen de alto nivel

AEM Los metadatos y las etiquetas son esenciales para aumentar la eficacia en la creación de. Los usuarios, los líderes y la administración se dan cuenta de la necesidad de una estrategia integral, pero les resulta difícil avanzar. A menudo, el conocimiento está aislado entre los usuarios, lo que dificulta la estrategia holística y hace que los ajustes sean aún más problemáticos.

¿Cuál es la diferencia entre metadatos y etiquetas? ¿Cuáles son los aspectos comerciales que se deben tener en cuenta al impulsar la estrategia?

## ¿Cuál es el propósito de los metadatos?

Los metadatos añaden estructura al contenido menos estructurado.
Ejemplo: una imagen básica tiene píxeles. Podemos llamar a esos &quot;datos básicos&quot;. Son los metadatos los que describen el formato, la categoría, los detalles de la licencia, etc.
Los metadatos se utilizan principalmente para los recursos. Sin embargo, hay un gran número de casos de uso para metadatos también en páginas de contenido o fragmentos de experiencias.

## Fuentes de metadatos

Las siguientes son las categorías de los metadatos que se pueden generar:

* Metadatos extraídos: la información ya está disponible en el documento, por ejemplo, en lenguaje natural.
* Metadatos derivados: la información no está disponible en los datos originales, pero se puede derivar haciendo referencias cruzadas a conocimientos anteriores.
* Metadatos añadidos manualmente: son metadatos que no entran en ninguna de las primeras categorías y que un humano debe añadir manualmente.

## Tipos de metadatos

Dentro de las categorías enumeradas anteriormente hay cuatro tipos principales:

* Metadatos técnicos y descriptivos: proporciona información sobre detalles técnicos del contenido (es decir, título, idioma, etc.)
* Metadatos operativos: Documenta el ciclo vital de un recurso (es decir, aprobado, en creativo, campaña)
* Metadatos administrativos: Estado de un recurso dentro de una organización (es decir, información de licencia, propiedad)
* Metadatos estructurales: ayuda a categorizar recursos o páginas para facilitar los procesos empresariales (se aplica a la mayoría de etiquetas y taxonomías)

## Nombres de carpeta y archivo

AEM Las carpetas son una forma natural de desplazarse por el contenido y explorarlo en las carpetas de la página de la página de la página de la página de la aplicación AEM ¿Cómo van a interactuar las partes interesadas con los? Esto determinará cómo se estructuran las carpetas. Normalmente, la estructura de carpetas se diseña teniendo en cuenta una (o dos) de las siguientes opciones:

* Navegación
* Exploración
* Categorización
* Control de acceso

Para AEM Sites, la navegación es clave. Las carpetas se utilizan para controlar el acceso a los recursos y a las páginas.

¿Qué niveles de autores van a necesitar acceso a páginas principales? ¿Qué sucede con las páginas de productos? ¿O campaña? Utilice permisos más estructura de carpetas para establecer el control correcto.

## Almacenamiento de metadatos

Existen tres métodos para almacenar metadatos:

* Binario: formato binario relacionado con la naturaleza del recurso (Photoshop, InDesign, PNG, JPG).
* Nodo de recurso: son metadatos del propio recurso, independientemente del sistema o proceso que se esté utilizando.
* Ubicación externa: Metadatos que no están directamente en el recurso, pero que pueden utilizarse como descriptor del &quot;estado&quot; de un recurso (por ejemplo: un flujo de trabajo que puede afectar a un recurso, pero no aplicárselo directamente)

## Modelo de metadatos

La estructura de cómo se capturan y se formatean los metadatos se denomina modelo de metadatos o esquema de metadatos. Esto se debe acordar antes de ingerir recursos o páginas en el sistema.

Un modelo de metadatos suele diseñarse para satisfacer los siguientes casos de uso:

* Buscar y recuperar: ayuda a almacenar aspectos clave del contenido que facilitan la recuperación por parte de la empresa.
* Reutilización: ayuda a utilizar recursos antiguos para su reutilización (ahorra tiempo y dinero).
* Administración de licencias: Rastrea la propiedad de los recursos por parte de la organización (a menudo por motivos legales)
* Distribuir: Poner el contenido a disposición de los consumidores o distribuir los activos a socios comerciales.
* Archivo: Los metadatos que registran un recurso están obsoletos (se recomienda siempre colocar un indicador &quot;archivado&quot; en el recurso para no perder información vital)/
* Referencia cruzada: Metadatos asociativos que capturan la relación de dos o más recursos entre sí (la síntesis de metadatos permite la referencia cruzada y la organización coherente del grupo)
* Navegar: la estructura de carpetas en la que se almacenan los recursos (utilizada para recuperar información mediante la exploración)

*Los metadatos de autor admiten principalmente procesos operativos. Publish admite casos de uso de recuperación y distribución.*

## Uso de etiquetas como términos predefinidos

Una etiqueta es una palabra clave o término asignado a un fragmento de información. Por ejemplo, en lugar de introducir &quot;coche&quot;, &quot;vehículo&quot;, &quot;automóvil&quot;, un sistema de etiquetas permite elegir un solo valor, lo que hace que la búsqueda sea más predecible.  Las etiquetas normalizan y simplifican la categorización de los recursos.

*AEM Nota: Aunque el etiquetado ad-hoc se permite, es recomendable no hacerlo, ya que podría generar una taxonomía indefinida y poco manejable.*

Usos comunes de las etiquetas:

* Búsqueda por palabras clave: una etiqueta puede describir que un recurso pertenece a un determinado grupo de entidades. Por ejemplo, una etiqueta &quot;image/subject/car&quot; describe que el recurso pertenece al conjunto de imágenes que muestran un coche.
* Relaciones guía: todos los recursos que comparten la misma etiqueta se pueden considerar conectados. El etiquetado, en lugar de la vinculación directa, es especialmente útil en los sitios web que tienen mucho contenido dinámico y conectado.
* Navegación de unidades: las etiquetas ordenadas en taxonomía jerárquica pueden generar navegación, un vínculo o a documentos similares.
Las etiquetas también deben ser información buscada que conecte varios tipos de datos, en función de términos comerciales y no de propiedades técnicas.

## Aplicaciones comunes de las etiquetas

AEM Cuando se utilizan en las etiquetas, las etiquetas pueden ayudar a lograr una implementación mucho más corta de la función compleja, como:

* Búsqueda con facetas
* Navegación personalizada
* Contenido relacionado
* Referencias de contenidos
* Optimización del motor de búsqueda
* Resaltar conceptos clave

## Taxonomías

Una taxonomía es un sistema de organización de etiquetas basado en características compartidas, que generalmente están estructuradas jerárquicamente según las necesidades de la organización. La estructura puede ayudar a encontrar una etiqueta más rápido o imponer una generalización.
Ejemplo: Es necesario subcategorizar las imágenes de stock de los automóviles.  La taxonomía podría tener el siguiente aspecto:

/subject/car/ /subject/car/sportscar /subject/car/sportscar/porsche /subject/car/sportscar/ferrari ... /subject/car/minivan /subject/car/minivan/mercedes /subject/car/minivan/volkswagen ... /subject/car/limousine ...

Ahora un usuario puede elegir si quiere buscar imágenes de cicatrices deportivas en general o de un &quot;Porsche&quot; en particular. Después de todo, ambas son cicatrices deportivas.
Práctica recomendada: Evite las taxonomías planas. Las taxonomías planas carecen de los beneficios descritos anteriormente y requieren un mantenimiento constante

**Uso de una taxonomía como sinónimos.**  Cuando un usuario busca una palabra clave, el sistema crea una segunda búsqueda de todos los sinónimos que se encuentran allí.
Además, en lugar de escribir &quot;car&quot; manualmente, el sistema puede proporcionar una lista de palabras clave para mejorar la coherencia.

**Uso de una taxonomía como diccionario.** En lugar de imprimir &quot;coche&quot;, puede expandir la etiqueta única y utilizar todos los sinónimos de la etiqueta.

**Varias categorías.** A diferencia de una jerarquía de carpetas, las etiquetas se pueden utilizar para expresar varias categorizaciones al mismo tiempo. Un recurso etiquetado con:

/subject/car/minivan/mercedes /subject/people/family /color/red

## Metadatos frente a etiqueta

No todos los metadatos deben considerarse como candidatos para el sistema de etiquetado. Los metadatos técnicos pueden, sin necesidad, duplicar la información. El mejor candidato para las etiquetas son los metadatos empresariales. Las etiquetas son una buena opción para aplicar un vocabulario, una búsqueda con facetas y una navegación coherentes.

## Administración de etiquetas

La administración de etiquetas se beneficia de un equipo principal dedicado. Los nuevos miembros deben conocer primero el propósito y la función de la taxonomía antes de agregar nuevas etiquetas.  Los expertos experimentados, que actúan como guardianes de las nuevas etiquetas, reducirán las inconsistencias a largo plazo.

## Creación de etiquetas

Las taxonomías deben ser empleadas por los autores de contenido y entendidas por los usuarios finales. Deben crearse antes del proceso de creación de contenido. Cualquier atajo resultará en un esfuerzo adicional para la administración y el mantenimiento.

## Mantenimiento en curso

Las cosas cambian, y las necesidades de la lista de etiquetas también. Cree un proceso de mantenimiento sólido que reduzca la duplicación.

Asegúrese de que los colaboradores de contenido sepan cómo pueden proponer cambios y de que los editores o los administradores de contenido revisen los términos regularmente.

## Prácticas recomendadas con etiquetas y taxonomías

**Estandarización de etiquetas.** Cree un glosario que proporcione un vocabulario autorizado. Si no se establecen normas, la duplicación planteará problemas. Además, se recomienda auditar no solo la taxonomía, sino también el uso de las etiquetas.

**No lo etiquetes en exceso.** Las etiquetas pueden perder su importancia si se distribuyen con demasiada frecuencia.Elimine las etiquetas superfluas para obtener una eficacia óptima.

**Volver a evaluar las etiquetas con el tiempo.** Recuerde que la terminología empresarial y el contexto empresarial rara vez permanecen estáticos. Es posible que necesite volver a estandarizar y aplicar las etiquetas.

**Uso del etiquetado inteligente con tecnología de IA.** Etiquetado inteligente [ver vínculo] AEM es una capacidad de IA en la que se puede reducir el esfuerzo de etiquetado manual de recursos. El etiquetado inteligente utiliza una IA para deducir información sobre el asunto de una imagen. Genera etiquetas descriptivas que describen el contenido de una imagen.

## Calidad y mantenimiento de metadatos

Comprender los requisitos empresariales es un paso importante para ejecutar un modelo de administración de metadatos. Sin definición, la información no se puede almacenar. Será necesario revisar el modelo periódicamente. Esta es una actividad de control de calidad vital.

Además, los metadatos deben capturarse lo antes posible en el proceso de creación de contenido. Si los metadatos no se &quot;aplican&quot; en el momento adecuado, hay pocas posibilidades de aplicarlos de forma retroactiva.

**Utilizar metadatos** para mejorar la colaboración: Utilice Adobe Asset Link, Adobe Bridge AEM y el escritorio de la aplicación para vincular el proceso creativo y utilice metadatos para optimizar los flujos de trabajo creativos. El uso de estas herramientas enriquece los metadatos y la experiencia del usuario en todo el proceso creativo.

## Prácticas recomendadas para la administración de metadatos

* Asigne un equipo principal con un fuerte mandato ejecutivo: forme un equipo principal de metadatos que tenga una comprensión completa del ecosistema empresarial y un fuerte mandato de la administración de la organización.
* Definir estrategia y administración de metadatos: una buena estrategia de metadatos puede ayudar a las organizaciones a explicar la necesidad y las ventajas de los metadatos.  Una estrategia consta de esquemas de metadatos, taxonomía, procesos empresariales (para la calidad y captura de datos), funciones y responsabilidades y procesos de gobernanza. *
* Definir y comunicar un modelo de metadatos coherente: la estrategia y el razonamiento definidos deben documentarse y comunicarse correctamente dentro de las organizaciones.
* Convención de nomenclatura estándar: Cree una convención de nomenclatura de archivos coherente y descriptiva para mejorar la promoción de la marca, la administración de la información y la facilidad de uso.
* Caracteres seguros en los nombres de archivo: los nombres de archivo deben poder ser interpretados por todos los sistemas operativos comunes. Puede utilizar caracteres, números, diéresis, espacios y subrayado con total seguridad. El símbolo menos también es seguro, pero si corta y pega, puede parecer un &quot;guion&quot;.
* AEM Convención de nomenclatura de versiones: ofrece algunas funcionalidades para mantener versiones anteriores de recursos. En algunos casos, es posible que desee conservar varias versiones. Sin embargo, debe asegurarse de que el esquema de asignación de versiones sea coherente.

## Metadatos organizativos frente a descriptivos

Algunas directrices pueden ayudarle a decidir cómo categorizar los metadatos:

**Descripción** : Si los datos describen el recurso o fragmento de contenido, deben formar parte de los metadatos adjuntos.

**Buscar** - Si los metadatos deben utilizarse en la búsqueda, deben adjuntarse.

**Exposición** : Si expone los metadatos de una plataforma de distribución a un tercero, asegúrese de no exponer también los metadatos &quot;internos&quot;.

**Duración** : cuanto más tiempo se supone que duren los metadatos, más probabilidades hay de que sean buenos candidatos para los metadatos adjuntos.

**Procesos comerciales relacionados** - Definitivamente es útil tener un ID de producto permanente como parte de los metadatos. Sin embargo, la categoría de un artículo en relación con el catálogo de productos es un metadato cuestionable para el recurso.

**Organización y procesamiento** : Si la naturaleza de los metadatos es de organización, como el estado de un flujo de trabajo de aprobación o la propiedad de un departamento determinado, los metadatos externos deben considerarse por encima de la asociación de los metadatos al recurso.

*Para crear la estrategia, haga las siguientes preguntas:*

* ¿Qué tipo de contenido e &quot;información adicional&quot; (= metadatos) se necesita para resolver problemas empresariales / preguntas comerciales / problemas empresariales?
* ¿Cuáles son las variables, los &quot;campos&quot; del esquema y qué son los valores posibles? Qué variables necesitan una entrada de texto libre, cuáles se pueden reducir por tipo (número, fecha, booleano, ...), un conjunto de valores fijos (por ejemplo, países) o etiquetas de una taxonomía determinada. ¿Cuántas etiquetas se requieren y se permiten?
* ¿Qué problemas técnicos / problemas / preguntas pueden resolverse con los metadatos?
* ¿Cómo se puede obtener/crear ese contenido/metadatos? ¿Cuánto costará obtener o crear esos metadatos?
* ¿Qué tipos de metadatos se necesitan para un grupo de usuarios en particular?
* ¿Cómo se mantendrán y actualizarán los metadatos?
* ¿Quién es el responsable de qué parte del proceso?
* ¿Cómo puede asegurarse de que se siguen los procesos empresariales acordados?
* ¿Qué estándares debe seguir? ¿Debería adoptar y modificar un estándar del sector (Dublin Core, ISO 19115, PRISM, etc.)? ¿o debería la organización crear sus propias normas?
* ¿Dónde se documenta la estrategia? ¿Cómo puede asegurarse de que todas las partes interesadas tengan acceso? ¿Cómo puede asegurarse de que el personal recién incorporado se adhiera al estándar acordado (por ejemplo, formación de visitas antes de obtener acceso)?
