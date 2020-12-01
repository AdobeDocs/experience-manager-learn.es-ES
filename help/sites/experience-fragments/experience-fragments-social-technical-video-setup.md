---
title: Configuración de anuncios sociales con fragmentos de experiencias AEM
description: Los fragmentos de experiencia permiten a los especialistas en marketing anunciar experiencias creadas en AEM en plataformas de medios sociales. El siguiente vídeo detalla la configuración necesaria para publicar fragmentos de experiencia en Facebook y Pinterest.
sub-product: sitios, servicios de contenido
feature: experience-fragments
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---


# Configurar anuncio social con fragmentos de experiencia {#set-up-social-posting-with-experience-fragments}

Los fragmentos de experiencia permiten a los especialistas en marketing anunciar experiencias creadas en AEM en plataformas de medios sociales. El siguiente vídeo detalla la configuración necesaria para publicar fragmentos de experiencia en Facebook y Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Fragmentos]  de experiencias: configuración para anuncios sociales en Facebook y Pinterest*

## Lista de comprobación para configurar los fragmentos de experiencia para publicarlos en Facebook y Pinterest

1. La instancia de AEM Author se está ejecutando en HTTPS
2. Cuenta de Facebook + aplicación para desarrolladores de Facebook
3. Pinterest Account + aplicación para desarrolladores de Pinterest
4. [!UICONTROL AEM Cloud ] ServicesConfiguración - Facebook
5. [!UICONTROL AEM Cloud ] ServicesConfiguración: Pinterest
6. Fragmento de experiencias AEM con Cloud Services para Facebook + Pinterest
7. Variación de fragmento de experiencia con plantilla de Facebook
8. Variación del fragmento de experiencias con la plantilla Pinterest

## URI de redirección de fragmento de experiencia

Este URI se utiliza para las aplicaciones de Facebook y Pinterest como parte del flujo de Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

