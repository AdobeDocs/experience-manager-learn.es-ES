---
title: Uso de CAPTCHA con AEM Forms adaptable
description: Adición y uso de un CAPTCHA con AEM Adaptive Forms.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 0%

---

# Uso de CAPTCHA con AEM Forms adaptable{#using-captchas-with-aem-adaptive-forms}

Adición y uso de un CAPTCHA con AEM Adaptive Forms.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Este vídeo explica el proceso de adición de un CAPTCHA a un formulario adaptable AEM mediante el servicio AEM CAPTCHA incorporado, así como el servicio reCAPTCHA de Google.*

>[!NOTE]
>
>Esta función solo está disponible a partir de AEM 6.3.

>[!NOTE]
>
>**Para configurar reCaptcha en la instancia de publicación, siga los pasos**
>
>Configurar reCaptach en la instancia de autor
>
>abrir el Felix [consola web](http://localhost:4502/system/console/bundles) en la instancia de autor
>
>buscar el paquete com.adobe.granite.crypto.file
>
>Tenga en cuenta el ID del paquete. En mi caso son 20
>
>Vaya al ID del paquete en el sistema de archivos de la instancia de autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copiar los archivos HMAC y maestro
Abra el [consola web felix](http://localhost:4502/system/console/bundles) en la instancia de publicación. Busque el paquete com.adobe.granite.crypto.file . Tenga en cuenta el ID del paquete
Vaya al ID del paquete en el sistema de archivos de la instancia de publicación
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* elimine los archivos HMAC y maestro existentes.
* pegar los archivos HMAC y maestro copiados de la instancia de autor

Reinicie el servidor de publicación de AEM

## Materiales de apoyo {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
