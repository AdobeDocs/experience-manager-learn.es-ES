---
title: Imágenes inteligentes
description: Las imágenes inteligentes en Dynamic Media Classic mejoran el rendimiento de la entrega de imágenes al optimizar automáticamente el formato y la calidad de las imágenes en función de las capacidades del navegador del cliente. Para ello, aprovecha las capacidades de Adobe Sensei AI y trabaja con ajustes preestablecidos de imagen existentes. Obtenga más información sobre imágenes inteligentes y cómo puede utilizarlas para ofrecer mejores experiencias al cliente mediante cargas de página más rápidas.
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 2%

---

# Imágenes inteligentes {#smart-imaging}

Uno de los aspectos más importantes de la experiencia del cliente en su sitio web, sitio móvil o aplicación es el tiempo de carga de la página. A menudo, los clientes abandonan un sitio o aplicación si una página tarda demasiado en cargarse. Las imágenes constituyen la mayoría del tiempo de carga de la página. Las imágenes inteligentes en Dynamic Media Classic mejoran el rendimiento de la entrega de imágenes al optimizar automáticamente el formato y la calidad de las imágenes en función de las capacidades del navegador del cliente. Para ello, aprovecha las capacidades de Adobe Sensei AI y trabaja con ajustes preestablecidos de imagen existentes. Las imágenes inteligentes reducen el tamaño de las imágenes en un 30 por ciento o más, lo que se traduce en una carga de página más rápida y mejores experiencias del cliente.

Las imágenes inteligentes también se benefician del aumento de rendimiento añadido que supone la integración total con el mejor servicio premium de Adobe. Este servicio encuentra la ruta óptima de Internet entre servidores, redes y puntos de interconexión que tiene la latencia más baja y/o la tasa de pérdida de paquetes que la ruta predeterminada de Internet.

Más información sobre [Imágenes inteligentes](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## Ventajas de las imágenes inteligentes

Dado que las imágenes constituyen la mayoría del tiempo de carga de una página, la mejora del rendimiento con imágenes inteligentes puede tener un impacto profundo en los KPI empresariales, como una mayor conversión, el tiempo invertido en el sitio y una menor tasa de devoluciones del sitio.

![imagen](assets/smart-imaging/smart-imaging-1.png)

## Funcionamiento de las imágenes inteligentes

Como se ha señalado anteriormente, Imágenes inteligentes aprovecha las capacidades de Adobe Sensei AI y funciona con ajustes preestablecidos de imagen existentes para convertir automáticamente imágenes a formatos de imagen de próxima generación óptimos, como WebP, manteniendo al mismo tiempo la fidelidad visual.

Más información sobre [Funcionamiento de las imágenes inteligentes](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), incluidos detalles como formatos de imagen admitidos (y qué sucede si no se utilizan estos formatos) y su impacto en los ajustes preestablecidos de imagen existentes que se están utilizando.

## Impacto de las imágenes inteligentes

Es probable que tenga que realizar cambios en las direcciones URL, los ajustes preestablecidos de imagen y el código del sitio para aprovechar las imágenes inteligentes. Si cumple los requisitos previos para utilizar imágenes inteligentes y solo está trabajando con imágenes en los formatos de imagen PNG y JPEG admitidos, no tiene que realizar ningún cambio.

Las imágenes inteligentes funcionan con imágenes entregadas a través de HTTP, HTTPS y HTTP/2.

>[!NOTE]
>
>Al pasar a Imágenes inteligentes, se borra la caché en la CDN. La caché en la CDN se suele crear de nuevo en uno o dos días.

Las imágenes inteligentes se incluyen en su licencia existente de Dynamic Media Classic. Esta función no conlleva costes adicionales. Para aprovecharlo, debe cumplir dos requisitos: tienen una CDN empaquetada en Adobe y un dominio dedicado. A continuación, debe activarlo para su cuenta porque no está activada automáticamente.

Al habilitar la imagen inteligente, se inicia con el envío de una solicitud al servicio de asistencia técnica de |creación de un caso de asistencia| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/es/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). La asistencia técnica le ayudará a configurar un dominio personalizado que asociará con las imágenes inteligentes. Cambiará un parámetro relacionado con el almacenamiento en caché (Time To Live, o TTL) y la compatibilidad borrará la caché. También puede realizar un paso de ensayo opcional si lo desea antes de pasar a producción. A continuación, cuando las imágenes inteligentes estén activadas, los clientes recibirán imágenes de menor tamaño, pero con la misma calidad que solicitaron. Esto significa que experimentan tiempos de carga de página más rápidos, y todo esto se hace automáticamente porque Adobe Sensei ayuda a elegir el tamaño más eficiente.

Una vez que haya habilitado las imágenes inteligentes, querrá comprobar que funcionan según lo esperado.

Probablemente tenga preguntas adicionales sobre imágenes inteligentes. Hemos compilado una lista de preguntas más frecuentes con respuestas. Lea el [Preguntas frecuentes](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html).

## Recursos adicionales

Observe el [Dynamic Media Classic Optimización del Generador de habilidades de rendimiento de la página](https://seminars.adobeconnect.com/pzc1gw0cihpv) seminario web a petición para obtener más información sobre imágenes inteligentes.
