---
title: Acelerar la velocidad de contenido con sistemas de estilos AEM
description: Aprenda a utilizar AEM sistemas de estilos para potenciar a los diseñadores, autores de contenido y desarrolladores de su organización a fin de crear y ofrecer experiencias a la velocidad y la escala que esperan sus clientes.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
source-git-commit: 471f0fe940abb8241428beb14896d83e140136b3
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 0%

---

# Acelerar la velocidad de contenido con sistemas de estilos AEM

En este artículo, aprenderá a utilizar sistemas de estilo AEM para potenciar a diseñadores, autores de contenido y desarrolladores de su organización a fin de crear y ofrecer experiencias a la velocidad y la escala que esperan sus clientes.

## Información general

AEM sistemas de estilos tienen cuatro ventajas clave:

* Los autores de plantillas pueden definir clases de estilos en la política de contenido de un componente o página
* Los autores de contenido pueden seleccionar estilos para aplicarlos a una página entera o al editar un componente en una página
* Los componentes y las plantillas se vuelven más flexibles ya que permiten a los autores presentar variaciones visuales alternativas
* La necesidad de desarrollar un componente personalizado o diálogos complejos para presentar variaciones de componentes se reduce o elimina completamente

## Configuración inicial y uso

La configuración de 5 pasos es muy similar a un flujo de trabajo de desarrollo de componentes estándar.

| **Liderazgo** | **Designer** | **Desarrollador/Arquitecto** | **Autor de plantillas** | **Autor de contenido** |
| --- | --- | --- | --- | --- |
| Determina el contenido y los objetivos de ese componente | Determina la presentación visual y experiencial del contenido | Desarrolla CSS y JS para admitir la experiencia; define y proporciona nombres de clase para usar | Configura las políticas de plantilla para los componentes con estilo mediante la adición de nombres de clase CSS definidos por los desarrolladores. Los nombres descriptivos deben aprovecharse para cada estilo. | Durante la creación de páginas, aplica los estilos necesarios para lograr el aspecto deseado |

Aunque esta es la configuración inicial, muchos de nuestros clientes han conseguido agilidad adicional al simplificar este proceso, por ejemplo, al cargar su CSS en DAM, que permite actualizar los estilos sin necesidad de implementación. Otros clientes tienen un conjunto completo de clases de utilidades, que les permite desarrollar componentes y estilos que se pueden aprovechar sin implementación ni desarrollo.

Los sistemas de estilos tienen algunos sabores diferentes:

1. Estilos de diseño

   * Cambios multifacéticos en el diseño y el diseño

   * Se utiliza para una representación bien definida e identificable

1. Mostrar estilos
   * Variaciones menores que no cambian la naturaleza fundamental del estilo

   * Por ejemplo, cambiar la combinación de colores, la fuente, la orientación de la imagen, etc.

1. Estilos informativos

   * Mostrar u ocultar campos

>[!NOTE]
>
>Para ver una demostración de estas funciones, le recomendamos que consulte nuestra [Seminario web de éxito del cliente](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) con Will Brisbane y Joseph Van Buskirk.

## Prácticas recomendadas

* Solidificar primero el estilo predeterminado
   * Diseño y visualización del componente cuando se suelta en la página antes de la aplicación de sistemas de estilos
   * Debe ser la representación más usada
* Intente mostrar solo las opciones de estilo que tengan un efecto cuando sea posible
   * Si se exponen combinaciones ineficaces, asegúrese de que no causan efectos negativos
   * Por ejemplo, un estilo de diseño que dicta la posición de la imagen y va acompañado de un estilo de visualización ineficaz que controla la posición de la imagen
* Opt para estilos de diseño en lugar de estilos de visualización combinados
   * Reduce el número de permutaciones que se deben comprobar de calidad
   * Garantiza el cumplimiento de los estándares de marca
   * Simplifica la creación para autores de contenido
   * Ayuda a crear una identidad de marca de sitio coherente
* Sea conservador con estilos combinados
   * Tanto dentro como fuera de categorías
* Asigne el tiempo adecuado para probar exhaustivamente los estilos combinados
   * Ayuda a evitar reacciones adversas
* Minimizar el número de opciones de estilo y permutaciones
   * Demasiadas opciones pueden provocar una falta de coherencia de la marca para que la apariencia sea correcta
   * Puede causar confusión para los autores de contenido en los que se necesitan combinaciones para lograr el efecto deseado
   * Aumenta las permutaciones que se deben comprobar de calidad
* Utilice etiquetas y categorías de estilo fáciles de usar para la empresa
   * &quot;Azul&quot; y &quot;Rojo&quot; en lugar de &quot;Primario&quot; y &quot;Secundario&quot;
   * &quot;Tarjeta&quot; y &quot;Hero&quot; en lugar de &quot;Variación A&quot; y &quot;Variación B&quot;
   * Esto puede ser más general para algunos clientes; el equipo de diseño, el equipo empresarial y el equipo de contenido están muy familiarizados con sus colores primarios y secundarios o con las variaciones que están probando. Pero para la flexibilidad y cualquier potencial de cambios futuros, usar términos específicos puede ser más eficiente.

## Principales tomas

Los sistemas de estilos reducen la necesidad de diálogos complejos, pero no sustituyen al diálogo. Ayudan a simplificar las cosas, pero puede haber algunos casos en los que desee utilizar propiedades de componentes o cuadro de diálogo en lugar de crear un sistema de estilos para él.

Pueden racionalizar los procesos desde una perspectiva de desarrollo. Puede conseguir múltiples aspectos del mismo contenido con un sistema de estilos. Del mismo modo, desde la perspectiva de la creación, en lugar de formar a los autores, y teniendo en cuenta qué componente utilizar en cada palacio, puede acelerar la velocidad de creación.

Las cosas son simplemente más limpias. El HTML dentro de los componentes principales es muy detallado. Todo esto a nivel de CSS hace que el componente se compila más rápido y el código también es más limpio.

Por último, el uso de Style Systems es más arte que ciencia. Como hemos analizado, existen varias prácticas recomendadas, pero tendrá flexibilidad para personalizar la configuración de su organización.

Para obtener más información, consulte nuestra [Seminario web de éxito del cliente](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) con Will Brisbane y Joseph Van Buskirk.

Obtenga más información sobre la estrategia y el liderazgo mental en [Éxito del cliente](https://experienceleague.corp.adobe.com/docs/customer-success/customer-success/overview.html) hub.
