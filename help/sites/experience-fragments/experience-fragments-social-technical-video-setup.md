---
title: Configurar publicación social con fragmentos de experiencia AEM
description: Los fragmentos de experiencias permiten a los especialistas en marketing publicar experiencias creadas en AEM en plataformas de medios sociales. El siguiente vídeo detalla la configuración necesaria para publicar fragmentos de experiencias en Facebook y Pinterest.
sub-product: sitios, servicios de contenido
feature: Fragmentos de experiencias
topics: integrations, content-delivery
audience: administrator, implementer, developer
doc-type: setup
activity: use
version: 6.3, 6.4, 6.5
topic: Administración de contenido
role: Admin, Developer
level: Intermediate
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 2%

---


# Configurar publicación social con fragmentos de experiencias {#set-up-social-posting-with-experience-fragments}

Los fragmentos de experiencias permiten a los especialistas en marketing publicar experiencias creadas en AEM en plataformas de medios sociales. El siguiente vídeo detalla la configuración necesaria para publicar fragmentos de experiencias en Facebook y Pinterest.

>[!VIDEO](https://video.tv.adobe.com/v/20592/?quality=9&learn=on)

*[Fragmentos de experiencias] : configuración para publicar en Facebook y Pinterest en redes sociales*

## Lista de comprobación para configurar fragmentos de experiencias para publicarlos en Facebook y Pinterest

1. La instancia de autor de AEM se ejecuta en HTTPS
2. Cuenta de facebook + aplicación para desarrolladores de Facebook
3. Cuenta de pinterest + aplicación para desarrolladores de Pinterest
4. [!UICONTROL Configuración de AEM Cloud ] Services - Facebook
5. [!UICONTROL Configuración de AEM Cloud ] Services - Pinterest
6. AEM fragmento de experiencia con Cloud Services para Facebook + Pinterest
7. Variación de fragmento de experiencia mediante una plantilla de Facebook
8. Variación de fragmento de experiencia mediante una plantilla de Pinterest

## URI de redirección de fragmento de experiencia

Este URI se utiliza para aplicaciones de Facebook y Pinterest como parte del flujo de Oauth.

```plain
 /* replace localhost:8443 with your aem host info */

 https://localhost:8443/libs/cq/experience-fragments/components/redirect
```

