---
title: Uso de la búsqueda de traducción inteligente con AEM Assets
description: AEM La búsqueda inteligente de traducciones permite la búsqueda y el descubrimiento en varios idiomas de forma automática en todos los contenidos, tanto de activos como de páginas, lo que admite más de 50 idiomas y reduce la necesidad de traducir contenido de forma manual.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
doc-type: Feature Video
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
duration: 195
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Uso de la búsqueda de traducción inteligente con AEM Assets{#using-smart-translation-search-with-aem-assets}

AEM La búsqueda inteligente de traducciones permite la búsqueda y el descubrimiento en varios idiomas de forma automática en todos los contenidos, tanto de activos como de páginas, lo que admite más de 50 idiomas y reduce la necesidad de traducir contenido de forma manual.

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

AEM AEM AEM La búsqueda de traducción inteligente permite a los usuarios realizar búsquedas de contenido en términos que no sean en inglés, para que coincidan con los recursos en los que los usuarios tienen términos en inglés equivalentes, y en los que los usuarios pueden realizar búsquedas de contenido en términos que no sean en inglés.

AEM La búsqueda inteligente de traducciones es un complemento perfecto para las etiquetas inteligentes de los recursos de la aplicación a los recursos en inglés.

Este vídeo da por hecho [AEM Búsqueda inteligente de traducción](smart-translation-search-technical-video-setup.md) se ha configurado.

## Funcionamiento de la búsqueda de traducción inteligente {#how-smart-translation-search-works}

![Diagrama de flujo de búsqueda de traducción inteligente](assets/smart-translation-search-flow.png)

1. AEM El usuario realiza una búsqueda de texto completo, proporcionando un término de búsqueda localizado (p. ej., el término español para &#39;hombre&#39;, &#39;hombre&#39;).
2. La búsqueda de traducción inteligente, proporcionada por el paquete OSGi de traducción automática de Apache Oak, se involucra y evalúa si los términos de búsqueda proporcionados se pueden traducir utilizando los paquetes de idiomas registrados.
3. Se recopilan todos los términos traducidos del paso #2 y la consulta se aumenta internamente para incluirlos como términos de búsqueda. AEM Este conjunto ampliado de términos de búsqueda si se evalúa normalmente frente a índices de búsqueda de la búsqueda de la búsqueda de la ubicación de coincidencias relevantes.
4. Los resultados de búsqueda que coinciden con el término original (&quot;hombre&quot;) o el término traducido (&quot;hombre&quot;) se recopilan y se devuelven al usuario como resultados de búsqueda.

## Recursos adicionales{#additional-resources}

* [Configuración de la búsqueda de traducción inteligente con AEM Assets](smart-translation-search-technical-video-setup.md)
* [Paquetes de idiomas de Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
