---
title: Uso de la búsqueda de traducción inteligente con AEM Assets
description: La búsqueda de traducción inteligente permite la búsqueda y la detección entre idiomas automáticamente en AEM contenido, tanto en Assets como en las páginas, admite más de 50 idiomas y reduce la necesidad de la traducción manual del contenido.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# Uso de la búsqueda de traducción inteligente con AEM Assets{#using-smart-translation-search-with-aem-assets}

La búsqueda de traducción inteligente permite la búsqueda y la detección entre idiomas automáticamente en AEM contenido, tanto en Assets como en las páginas, admite más de 50 idiomas y reduce la necesidad de la traducción manual del contenido.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM búsqueda de traducción inteligente permite a los usuarios realizar búsquedas de contenido en AEM utilizando términos que no estén en inglés, para que coincidan con los recursos de AEM que tengan términos en inglés equivalentes.

La búsqueda de traducción inteligente es un complemento perfecto para AEM etiquetas inteligentes que se aplican a los recursos en inglés.

Este vídeo asume [Búsqueda de traducción inteligente AEM](smart-translation-search-technical-video-setup.md) se ha configurado.

## Funcionamiento de la búsqueda de traducción inteligente {#how-smart-translation-search-works}

![Diagrama de flujo de búsqueda de traducción inteligente](assets/smart-translation-search-flow.png)

1. AEM usuario realiza una búsqueda de texto completo, proporcionando un término de búsqueda localizado (p. ej. el término español &quot;hombre&quot;, &quot;hombre&quot;).
2. La búsqueda de traducción inteligente, proporcionada por el paquete OSGi de Apache Oak Machine Translation, participa y evalúa si los términos de búsqueda proporcionados se pueden traducir utilizando los paquetes de idiomas registrados.
3. Se recopilan todos los términos traducidos del Paso 2 y la consulta se incrementa internamente para incluirlos como términos de búsqueda. Este conjunto aumentado de términos de búsqueda si se evalúan normalmente en relación con AEM índices de búsqueda que localizan coincidencias relevantes.
4. Los resultados de búsqueda que coincidan con el término original (&quot;hombre&quot;) o el término traducido (&quot;hombre&quot;) se recopilarán y devolverán al usuario como resultados de la búsqueda.

## Recursos adicionales{#additional-resources}

* [Configuración de la búsqueda de traducción inteligente con AEM Assets](smart-translation-search-technical-video-setup.md)
* [Paquetes de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
