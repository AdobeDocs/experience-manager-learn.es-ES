---
title: Uso de CAPTCHA con Forms adaptable de AEM
description: Agregar y usar un CAPTCHA con un Forms adaptable de AEM.
feature: Adaptive Forms,Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 260
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Uso de CAPTCHA con Forms adaptable de AEM{#using-captchas-with-aem-adaptive-forms}

Agregar y usar un CAPTCHA con un Forms adaptable de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/39205?quality=12&learn=on&captions=spa)

*Este vídeo muestra el proceso de agregar un CAPTCHA a un formulario adaptable de AEM mediante el servicio AEM CAPTCHA integrado y el servicio reCAPTCHA de Google.*

>[!NOTE]
>
>Esta función solo está disponible a partir de AEM 6.3.

>[!NOTE]
>
>**Para configurar reCaptcha en la instancia de publicación, siga los pasos**
>
>Configurar reCaptach en la instancia de autor
>
>abra la [consola web](http://localhost:4502/system/console/bundles) de Felix en la instancia de autor
>
>buscar el paquete com.adobe.granite.crypto.file
>
>Anote el ID del paquete. En mi instancia son 20
>
>Vaya al ID del paquete en el sistema de archivos de la instancia de autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* Copiar los archivos HMAC y maestro
>
>Abra la consola web [felix](http://localhost:4502/system/console/bundles) en la instancia de publicación. Busque el paquete com.adobe.granite.crypto.file. Anote el ID del paquete
>
>Vaya al ID del paquete en el sistema de archivos de la instancia de publicación
>
>* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* elimine los archivos HMAC y maestros existentes.
>* pegue los archivos HMAC y maestro copiados de la instancia de autor
>
>Reinicie el servidor de publicación de AEM

## Materiales de apoyo {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
