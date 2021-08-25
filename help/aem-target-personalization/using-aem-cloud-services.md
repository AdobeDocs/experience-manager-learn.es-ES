---
title: Integración de Adobe Experience Manager con Adobe Target mediante Cloud Services
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: Introducción paso a paso sobre cómo integrar Adobe Experience Manager (AEM) con Adobe Target mediante AEM Cloud Service
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 3%

---


# Uso de Cloud Services heredados de AEM

En esta sección, analizaremos cómo configurar Adobe Experience Manager (AEM) con Adobe Target mediante Cloud Services heredados.

>[!NOTE]
>
> El Cloud Service AEM heredado con Adobe Target se **solo** utiliza para establecer una conexión back-end directa de AEM Author a Adobe Target, lo que facilita la publicación de contenido de AEM a Target. Adobe Launch se utiliza para exponer a Adobe Target en la experiencia del sitio web pública que AEM.

Para utilizar AEM ofertas de fragmentos de experiencia con el fin de potenciar sus actividades de personalización, pasemos al capítulo siguiente e integremos AEM con Adobe Target mediante los servicios de nube heredados. Esta integración es necesaria para insertar los fragmentos de experiencias de AEM a Target como ofertas HTML/JSON y para mantener las ofertas de destino sincronizadas con AEM. Esta integración es necesaria para implementar el [Escenario 1 que se describe en la sección de información general](./overview.md#personalization-using-aem-experience-fragment).

## Requisitos previos

* **AEM**

   * AEM instancia de creación y publicación son necesarias para completar este tutorial. Si todavía no ha configurado la instancia de AEM, puede seguir los pasos [aquí](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > El cliente debe estar aprovisionado con el Experience Platform Launch y el Adobe I/O de [Adobe support](https://helpx.adobe.com/es/contact/enterprise-support.ec.html) o ponerse en contacto con el administrador del sistema


### Integración de AEM con Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Crear un Cloud Service de Adobe Target mediante la autenticación IMS de Adobe (*Utiliza la API de Adobe Target*) (00:34)
2. Obtener código de cliente de Adobe Target (1:50)
3. Creación de la configuración de IMS de Adobe para Adobe Target (02:08)
4. Crear una cuenta técnica para acceder a la API de Target dentro de la consola de Adobe I/O (02:08)
5. Agregar un Cloud Service de Adobe Target a AEM fragmentos de experiencias (04:12)

En este punto, ha integrado correctamente [AEM con Adobe Target mediante Cloud Services heredados](./using-aem-cloud-services.md#integrating-aem-target-options) como se detalla en la Opción 2. Ahora debería poder crear un fragmento de experiencia dentro de AEM y publicar el fragmento de experiencia como oferta HTML u oferta JSON en Adobe Target, y luego utilizarlo para crear una actividad.
