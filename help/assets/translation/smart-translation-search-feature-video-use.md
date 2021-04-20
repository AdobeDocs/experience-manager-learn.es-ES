---
title: Uso de la búsqueda de traducción inteligente con AEM Assets
description: La búsqueda de traducción inteligente permite la búsqueda y la detección entre idiomas automáticamente en todo el contenido de AEM, tanto en Assets como en Páginas, admite más de 50 idiomas y reduce la necesidad de la traducción manual del contenido.
version: 6.3, 6.4, 6.5
feature: Search
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# Uso de la búsqueda de traducción inteligente con AEM Assets{#using-smart-translation-search-with-aem-assets}

La búsqueda de traducción inteligente permite la búsqueda y la detección entre idiomas automáticamente en todo el contenido de AEM, tanto en Assets como en Páginas, admite más de 50 idiomas y reduce la necesidad de la traducción manual del contenido.

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

La búsqueda de traducción inteligente de AEM permite a los usuarios realizar búsquedas de contenido en AEM utilizando términos que no estén en inglés, para que coincidan con los recursos de AEM que tienen términos equivalentes en inglés.

La búsqueda de traducción inteligente es un complemento perfecto para las etiquetas inteligentes de AEM que se aplican a los recursos en inglés.

Este vídeo supone que se ha configurado [AEM Smart Translation Search](smart-translation-search-technical-video-setup.md).

## Funcionamiento de la búsqueda de traducción inteligente {#how-smart-translation-search-works}

![Diagrama de flujo de búsqueda de traducción inteligente](assets/smart-translation-search-flow.png)

1. El usuario de AEM realiza una búsqueda de texto completo, proporcionando un término de búsqueda localizado (p. ej. el término español &quot;hombre&quot;, &quot;hombre&quot;).
2. La búsqueda de traducción inteligente, proporcionada por el paquete OSGi de Apache Oak Machine Translation, participa y evalúa si los términos de búsqueda proporcionados se pueden traducir utilizando los paquetes de idiomas registrados.
3. Se recopilan todos los términos traducidos del Paso 2 y la consulta se incrementa internamente para incluirlos como términos de búsqueda. Este conjunto aumentado de términos de búsqueda si se evalúa normalmente en relación con los índices de búsqueda de AEM que encuentran coincidencias relevantes.
4. Los resultados de búsqueda que coincidan con el término original (&quot;hombre&quot;) o el término traducido (&quot;hombre&quot;) se recopilarán y devolverán al usuario como resultados de la búsqueda.

## Recursos adicionales{#additional-resources}

* [Configuración de la búsqueda de traducción inteligente con AEM Assets](smart-translation-search-technical-video-setup.md)
* [Paquetes de idioma Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)