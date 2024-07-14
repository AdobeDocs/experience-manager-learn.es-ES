---
title: AEM Acelerar la velocidad del contenido con sistemas de estilos de
description: AEM Aprenda a utilizar los sistemas de estilos de para que los diseñadores, los autores de contenido y los desarrolladores de su organización puedan crear y ofrecer experiencias a la velocidad y a la escala que esperan sus clientes.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 0%

---

# AEM Acelerar la velocidad del contenido con sistemas de estilos de

AEM En este artículo, aprenderá a utilizar sistemas del estilo de la para permitir a los diseñadores, autores de contenido y desarrolladores de su organización crear y ofrecer experiencias a la velocidad y a la escala que esperan sus clientes.

## Información general

AEM Los sistemas de estilo de la tienen cuatro ventajas clave:

* Los autores de plantillas pueden definir clases de estilo en la política de contenido de un componente o página
* Los autores de contenido pueden seleccionar estilos para aplicarlos a una página completa o al editar un componente de una página
* Los componentes y las plantillas se hacen más flexibles al permitir a los autores procesar variaciones visuales alternativas
* La necesidad de desarrollar un componente personalizado o diálogos complejos para presentar las variaciones de componentes se reduce o elimina por completo

## Configuración y uso iniciales

La configuración de 5 pasos es muy similar a un flujo de trabajo de desarrollo de componentes estándar.

| **Liderazgo** | **Designer** | **Desarrollador / Arquitecto** | **Autor de plantillas** | **Autor de contenido** |
| --- | --- | --- | --- | --- |
| Determina el contenido y los objetivos de ese componente | Determina la presentación visual y experiencial del contenido | Desarrolla CSS y JS para admitir la experiencia; define y proporciona nombres de clase para utilizar | Configura las políticas de plantilla para los componentes con estilo añadiendo nombres de clase CSS definidos por los desarrolladores. Los nombres descriptivos deben aprovecharse para cada estilo. | Durante la creación de páginas, aplica los estilos según sea necesario para lograr la apariencia deseada |

Aunque esta es la configuración inicial, muchos de nuestros clientes han conseguido una agilidad adicional optimizando este proceso, por ejemplo cargando su CSS en el DAM, lo que permite actualizar los estilos sin necesidad de implementación. Otros clientes tienen un conjunto completo de clases de utilidades, que les permite desarrollar componentes y estilos que luego se pueden aprovechar sin implementación ni desarrollo.

Los sistemas de estilos vienen en algunos sabores diferentes:

1. Estilos de diseño

   * Cambios multifacéticos en el diseño y la presentación

   * Se utiliza para representaciones bien definidas e identificables

1. Estilos de visualización
   * Variaciones menores que no cambian la naturaleza fundamental del estilo

   * Por ejemplo, cambiar el esquema de colores, la fuente, la orientación de la imagen, etc.

1. Estilos informativos

   * Mostrar u ocultar campos

>[!NOTE]
>
>Para ver una demostración de estas características, le recomendamos ver nuestro [seminario web sobre el éxito del cliente](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) con Will Brisbane y Joseph Van Buskirk.

## Prácticas recomendadas

* Solidificar primero el estilo predeterminado
   * Diseño y visualización del componente al colocarlo en la página antes de la aplicación de sistemas de estilos
   * Esta debe ser la representación más utilizada
* Intente mostrar únicamente las opciones de estilo que tienen un efecto cuando sea posible
   * Si se exponen combinaciones ineficaces, asegúrese de que no causen efectos negativos
   * Por ejemplo, un estilo de diseño que dicta la posición de la imagen y va acompañado de un estilo de visualización ineficaz que controla la posición de la imagen
* Optar por estilos de diseño en lugar de estilos de visualización combinados
   * Reduce el número de permutaciones que deben comprobarse con calidad
   * Garantiza el cumplimiento de los estándares de marca
   * Simplifica la creación para autores de contenido
   * Ayuda a crear una identidad de marca de sitio coherente
* Sea conservador con los estilos combinados
   * Tanto entre categorías como dentro de ellas
* Asigne el tiempo adecuado para probar a fondo los estilos combinados
   * Ayuda a evitar efectos no deseados
* Minimice el número de opciones de estilo y permutaciones
   * Demasiadas opciones pueden provocar una falta de coherencia de la marca para el aspecto
   * Puede causar confusión a los autores de contenido sobre qué combinaciones son necesarias para lograr el efecto deseado
   * Aumenta las permutaciones que deben comprobarse con calidad
* Utilice etiquetas y categorías de estilo empresarial fáciles de usar
   * &quot;Azul&quot; y &quot;Rojo&quot; en lugar de &quot;Principal&quot; y &quot;Secundario&quot;
   * &quot;Tarjeta&quot; y &quot;Hero&quot; en lugar de &quot;Variación A&quot; y &quot;Variación B&quot;
   * Esto puede ser más una generalidad para algunos clientes; el equipo de diseño, el equipo empresarial y el equipo de contenido están muy familiarizados con cuáles son sus colores primarios y secundarios o con qué variaciones están probando. Pero para la flexibilidad y cualquier potencial de cambios futuros, el uso de términos específicos puede ser más eficiente.

## Puntos clave

Los sistemas de estilos reducen la necesidad de diálogos complejos, pero no sustituyen a los diálogos. Ayudan a simplificar las cosas, pero puede haber casos en los que desee utilizar propiedades de componentes o cuadros de diálogo en lugar de crear un sistema de estilos para ellos.

Pueden agilizar los procesos desde la perspectiva del desarrollo. Puede lograr varios aspectos del mismo contenido con un sistema de estilos. Del mismo modo, desde una perspectiva de creación, en lugar de formar autores, y los autores que tienen que recordar qué componente utilizar en qué palacio, se puede acelerar la velocidad de creación.

Las cosas son simplemente más limpias. El HTML dentro de los componentes principales es muy detallado. Hacer todo esto en el nivel CSS hace que el componente se cree más rápido y que el código también sea más limpio.

Por último, el uso de los sistemas de estilos es más arte que ciencia. Como hemos comentado, hay una serie de prácticas recomendadas, pero tendrá flexibilidad para personalizar la configuración de su organización.

Para obtener más información, vea nuestro [Seminario web sobre el éxito de los clientes](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) con Will Brisbane y Joseph Van Buskirk.

Obtenga más información sobre estrategia y liderazgo mental en el centro de [Éxito del cliente](https://experienceleague.adobe.com/docs/customer-success/customer-success/overview.html).
