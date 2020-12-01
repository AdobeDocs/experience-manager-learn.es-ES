---
title: Uso de CAPTCHA con AEM Forms adaptable
seo-title: Uso de CAPTCHA con AEM Forms adaptable
description: Añadir y utilizar un CAPTCHA con AEM Forms adaptable.
seo-description: Añadir y utilizar un CAPTCHA con AEM Forms adaptable.
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---


# Uso de CAPTCHA con AEM Forms adaptable{#using-captchas-with-aem-adaptive-forms}

Añadir y utilizar un CAPTCHA con AEM Forms adaptable.

Visite la página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para obtener un vínculo a una demostración en directo de esta capacidad.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Este vídeo explica el proceso de adición de un CAPTCHA a un formulario adaptable AEM mediante el servicio AEM CAPTCHA incorporado y el servicio reCAPTCHA de Google.*

>[!NOTE]
>
>Esta función solo está disponible a partir de AEM 6.3.

>[!NOTE]
>
>**Para configurar reCaptcha en la instancia de publicación, siga los pasos**
>
>Configurar reCaptach en la instancia de autor
>
>abra la consola web felix [](http://localhost:4502/system/console/bundles) en la instancia del autor
>
>buscar el paquete com.adobe.granite.crypto.file
>
>Tenga en cuenta el ID del paquete. En mi caso son 20
>
>Vaya al ID del paquete en el sistema de archivos de la instancia de creación
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copiar los archivos principales y HMAC

Abra la [consola web felix](http://localhost:4502/system/console/bundles) en la instancia de publicación. Busque el paquete com.adobe.granite.crypto.file. Tenga en cuenta el ID del paquete
Vaya al ID del paquete en el sistema de archivos de la instancia de publicación
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* elimine los archivos principales y HMAC existentes.
* pegar los archivos principales y HMAC copiados de la instancia de creación

Reinicie el servidor de publicación AEM

## Materiales de apoyo {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

