---
title: Uso de la búsqueda de traducción inteligente con AEM Assets
seo-title: Uso de la búsqueda de traducción inteligente con AEM Assets
description: La búsqueda de traducción inteligente permite la búsqueda y detección en varios idiomas automáticamente en AEM contenido, tanto en Recursos como en Páginas, admite más de 50 idiomas y reduce la necesidad de traducción manual del contenido.
seo-description: La búsqueda de traducción inteligente permite la búsqueda y detección en varios idiomas automáticamente en AEM contenido, tanto en Recursos como en Páginas, admite más de 50 idiomas y reduce la necesidad de traducción manual del contenido.
uuid: daa6f20f-a4d3-402d-83b9-57d852062a89
discoiquuid: eb2e484a-0068-458f-acff-42dd95a40aab
topics: authoring, search, metadata, localization
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Uso de la búsqueda de traducción inteligente con AEM Assets{#using-smart-translation-search-with-aem-assets}

La búsqueda de traducción inteligente permite la búsqueda y detección en varios idiomas automáticamente en AEM contenido, tanto en Recursos como en Páginas, admite más de 50 idiomas y reduce la necesidad de traducción manual del contenido.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM búsqueda de traducción inteligente permite a los usuarios realizar búsquedas de contenido en AEM utilizando términos que no sean inglés, para que coincidan con los recursos de AEM que tienen términos equivalentes en inglés.

La búsqueda de traducción inteligente es un complemento perfecto para AEM etiquetas inteligentes que se aplican a los recursos en inglés.

Este vídeo supone que se ha configurado [AEM búsqueda](smart-translation-search-technical-video-setup.md) de traducción inteligente.

## Funcionamiento de la búsqueda de traducción inteligente {#how-smart-translation-search-works}

![Diagrama de flujo de búsqueda de traducción inteligente](assets/smart-translation-search-flow.png)

1. AEM usuario realiza una búsqueda de texto completo, proporcionando un término de búsqueda localizado (por ejemplo: el término español &quot;hombre&quot;, &quot;hombre&quot;).
2. La búsqueda de traducción inteligente, proporcionada por el paquete OSGi de traducción de Apache Oak Machine, se involucra y evalúa si los términos de búsqueda proporcionados pueden traducirse usando los paquetes de idioma registrados.
3. Se recopilan todos los términos traducidos del paso 2 y la consulta se aumenta internamente para incluirlos como términos de búsqueda. Este conjunto aumentado de términos de búsqueda si se evalúan normalmente en relación con índices de búsqueda AEM que buscan coincidencias relevantes.
4. Los resultados de búsqueda que coinciden con el término original (&#39;hombre&#39;) o el término traducido (&#39;hombre&#39;) se recopilan y devuelven al usuario como resultados de búsqueda.

## Recursos adicionales{#additional-resources}

* [Configurar la búsqueda de traducción inteligente con AEM Assets](smart-translation-search-technical-video-setup.md)
* [Paquetes de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)