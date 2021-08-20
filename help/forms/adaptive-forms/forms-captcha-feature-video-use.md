---
title: Uso de CAPTCHA con AEM Forms adaptable
description: Adición y uso de un CAPTCHA con AEM Adaptive Forms.
feature: Forms adaptable,Flujo de trabajo
version: 6.4,6.5
topic: Desarrollo
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# Uso de CAPTCHA con AEM Forms adaptable{#using-captchas-with-aem-adaptive-forms}

Adición y uso de un CAPTCHA con AEM Adaptive Forms.

Visite la página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse1) para ver un vínculo a una demostración en directo de esta capacidad.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Este vídeo explica el proceso de adición de un CAPTCHA a un formulario adaptable AEM utilizando tanto el servicio AEM CAPTCHA integrado como el servicio reCAPTCHA de Google.*

>[!NOTE]
>
>Esta función solo está disponible a partir de AEM 6.3.

>[!NOTE]
>
>**Para configurar reCaptcha en la instancia de publicación, siga los pasos**
>
>Configurar reCaptach en la instancia de autor
>
>abra la consola web Felix [](http://localhost:4502/system/console/bundles) en la instancia de autor
>
>buscar el paquete com.adobe.granite.crypto.file
>
>Tenga en cuenta el ID del paquete. En mi caso son 20
>
>Vaya al ID del paquete en el sistema de archivos de la instancia de autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copiar los archivos HMAC y maestro

Abra la [consola web felix](http://localhost:4502/system/console/bundles) en la instancia de publicación. Busque el paquete com.adobe.granite.crypto.file . Tenga en cuenta el ID del paquete
Vaya al ID del paquete en el sistema de archivos de la instancia de publicación
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* elimine los archivos HMAC y maestro existentes.
* pegar los archivos HMAC y maestro copiados de la instancia de autor

Reinicie el servidor de publicación de AEM

## Materiales de apoyo {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

