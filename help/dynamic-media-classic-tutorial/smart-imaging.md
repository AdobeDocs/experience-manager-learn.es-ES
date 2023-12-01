---
title: Imágenes inteligentes
description: Imágenes inteligentes en Dynamic Media Classic mejora el rendimiento de la entrega de imágenes al optimizar automáticamente el formato y la calidad de las imágenes en función de las funciones del explorador del cliente. Para ello, aprovecha las capacidades de IA de Adobe Sensei y trabaja con los ajustes preestablecidos de imagen existentes. Obtenga más información sobre imágenes inteligentes y cómo puede utilizarlas para ofrecer mejores experiencias de cliente mediante cargas de página más rápidas.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 2%

---

# Imágenes inteligentes {#smart-imaging}

Uno de los aspectos más importantes de la experiencia del cliente en su sitio web, sitio móvil o aplicación es el tiempo de carga de la página. Los clientes suelen abandonar un sitio o una aplicación si una página tarda demasiado en cargarse. Las imágenes constituyen la mayoría del tiempo de carga de la página. Imágenes inteligentes en Dynamic Media Classic mejora el rendimiento de la entrega de imágenes al optimizar automáticamente el formato y la calidad de las imágenes en función de las funciones del explorador del cliente. Para ello, aprovecha las capacidades de IA de Adobe Sensei y trabaja con los ajustes preestablecidos de imagen existentes. Imágenes inteligentes reduce el tamaño de las imágenes en un 30 % o más, lo que se traduce en cargas de página más rápidas y mejores experiencias para los clientes.

Las imágenes inteligentes también se benefician del aumento de rendimiento añadido de estar totalmente integradas con el mejor servicio premium de su clase de Adobe. Este servicio encuentra la ruta de Internet óptima entre servidores, redes y puntos de intercambio entre iguales que tiene la menor latencia o tasa de pérdida de paquetes que la ruta predeterminada de Internet.

Más información sobre [Imágenes inteligentes](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/dm-general-settings.html?lang=es).

## Ventajas de imágenes inteligentes

Dado que las imágenes constituyen la mayor parte del tiempo de carga de una página, la mejora del rendimiento de Imágenes inteligentes puede tener un profundo impacto en los KPI empresariales, como una mayor conversión, el tiempo invertido en el sitio y una tasa de salida hacia otro sitio más baja.

![imagen](assets/smart-imaging/smart-imaging-1.png)

## Funcionamiento de las imágenes inteligentes

Como se ha indicado anteriormente, Imágenes inteligentes aprovecha las capacidades de IA de Adobe Sensei y funciona con los ajustes preestablecidos de imagen existentes para convertir automáticamente imágenes a formatos de imagen óptimos de próxima generación, como WebP, a la vez que mantiene la fidelidad visual.

Más información sobre [Funcionamiento de las imágenes inteligentes](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=es), incluidos detalles como los formatos de imagen admitidos (y qué sucede si no utiliza esos formatos) y su impacto en los ajustes preestablecidos de imagen existentes que se están utilizando.

## Impactos de las imágenes inteligentes

Es probable que le preocupe tener que realizar cambios en las direcciones URL, los ajustes preestablecidos de imagen y el código del sitio para aprovechar las ventajas de las imágenes inteligentes. Si cumple los requisitos previos para utilizar imágenes inteligentes y solo está trabajando con imágenes en los formatos de imagen JPEG y PNG admitidos, no tiene que realizar ningún cambio.

Las imágenes inteligentes funcionan con imágenes entregadas a través de HTTP, HTTPS y HTTP/2.

>[!NOTE]
>
>Al pasar a Imágenes inteligentes, se borra la caché en la CDN. La caché de la CDN suele volver a crearse en un plazo de uno o dos días.

Las imágenes inteligentes se incluyen con la licencia existente de Dynamic Media Classic. No hay costes adicionales para esta función. Para aprovecharlo, debe cumplir dos requisitos: tener una CDN agrupada en Adobes y un dominio dedicado. A continuación, debe habilitarlo para su cuenta, ya que no se habilita automáticamente.

La activación de imágenes inteligentes comienza con el envío al servicio de asistencia técnica de una solicitud de |creación de un caso de asistencia| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/es/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). La asistencia técnica trabajará con usted para configurar un dominio personalizado que asociará con Imágenes inteligentes. Cambiará un parámetro relacionado con el almacenamiento en caché (Tiempo de vida o TTL) y la compatibilidad borrará la caché. También puede realizar un paso de ensayo opcional si lo desea antes de pasar a producción. A continuación, cuando se activa Smart Imaging, se ofrecen a los clientes imágenes de menor tamaño, pero con la misma calidad que solicitaban. Esto significa que experimentan tiempos de carga de página más rápidos, y todo esto se realiza automáticamente porque Adobe Sensei ayuda a elegir el tamaño más eficiente.

Una vez que haya habilitado Imágenes inteligentes, querrá comprobar que funciona según lo esperado.

Probablemente tenga más preguntas sobre Imágenes inteligentes. Hemos compilado una lista de preguntas frecuentes (FAQ) con respuestas. Lea el [Preguntas frecuentes](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/dm-publish-settings.html?lang=es).

## Recursos adicionales

Vea la [Dynamic Media Classic Optimización del rendimiento de la página Generador de habilidades](https://seminars.adobeconnect.com/pzc1gw0cihpv) Seminario web bajo demanda para obtener más información sobre imágenes inteligentes.
