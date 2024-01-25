---
title: AEM Uso de CAPTCHA con Forms adaptable de la
description: AEM Adición y uso de un CAPTCHA con Forms adaptable de.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 272
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# AEM Uso de CAPTCHA con Forms adaptable de la{#using-captchas-with-aem-adaptive-forms}

AEM Adición y uso de un CAPTCHA con Forms adaptable de.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*AEM AEM Este vídeo muestra el proceso de agregar un CAPTCHA a un formulario adaptable de la mediante el servicio integrado de CAPTCHA, así como el servicio reCAPTCHA de Google.*

>[!NOTE]
>
>AEM Esta función solo está disponible a partir de la versión 6.3 de la versión de.

>[!NOTE]
>
>**Para configurar reCaptcha en una instancia de publicación, siga los pasos**
>
>Configurar reCaptach en la instancia de autor
>
>abra el Felix [consola web](http://localhost:4502/system/console/bundles) en la instancia de autor
>
>buscar el paquete com.adobe.granite.crypto.file
>
>Anote el ID del paquete. En mi instancia son 20
>
>Vaya al ID del paquete en el sistema de archivos de la instancia de autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copiar los archivos HMAC y maestro
>
Abra el [consola web felix](http://localhost:4502/system/console/bundles) en la instancia de publicación. Busque el paquete com.adobe.granite.crypto.file. Anote el ID del paquete
>
Vaya al ID del paquete en el sistema de archivos de la instancia de publicación
>
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* elimine los archivos HMAC y maestros existentes.
* pegue los archivos HMAC y maestro copiados de la instancia de autor
>
AEM Reinicie el servidor de publicación de la

## Materiales de apoyo {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
