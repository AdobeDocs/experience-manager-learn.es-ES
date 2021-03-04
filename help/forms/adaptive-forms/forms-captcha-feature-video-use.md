---
title: Uso de CAPTCHA con AEM Adaptive Forms
seo-title: Uso de CAPTCHA con AEM Adaptive Forms
description: Adición y uso de un CAPTCHA con AEM Adaptive Forms.
seo-description: Adición y uso de un CAPTCHA con AEM Adaptive Forms.
feature: formularios adaptables
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---


# Uso de CAPTCHA con AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

Adición y uso de un CAPTCHA con AEM Adaptive Forms.

Visite la página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para ver un vínculo a una demostración en directo de esta capacidad.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Este vídeo explica el proceso de adición de un CAPTCHA a un formulario adaptable de AEM mediante el servicio AEM CAPTCHA incorporado, así como el servicio reCAPTCHA de Google.*

>[!NOTE]
>
>Esta función solo está disponible con AEM 6.3 y posteriores.

>[!NOTE]
>
>**Para configurar reCaptcha en la instancia de publicación, siga los pasos**
>
>Configurar reCaptach en la instancia de autor
>
>abra la consola web felix [web](http://localhost:4502/system/console/bundles) en la instancia de autor
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

## Materiales de soporte {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

