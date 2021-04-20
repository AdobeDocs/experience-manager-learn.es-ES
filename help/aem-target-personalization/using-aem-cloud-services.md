---
title: Integración de Adobe Experience Manager con Adobe Target mediante Cloud Services
seo-title: Integración de Adobe Experience Manager (AEM) con Adobe Target mediante servicios de nube heredados
description: Introducción paso a paso sobre cómo integrar Adobe Experience Manager (AEM) con Adobe Target mediante AEM Cloud Service
seo-description: Introducción paso a paso sobre cómo integrar Adobe Experience Manager (AEM) con Adobe Target mediante AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 4%

---


# Uso de servicios de nube heredados de AEM

En esta sección, analizaremos cómo configurar Adobe Experience Manager (AEM) con Adobe Target mediante servicios de nube heredados.

>[!NOTE]
>
> El servicio en la nube heredado de AEM con Adobe Target **solo** se utiliza para establecer una conexión back-end directa de AEM Author a Adobe Target, lo que facilita la publicación de contenido de AEM a Target. Adobe Launch se utiliza para exponer Adobe Target en la experiencia del sitio web público ofrecida por AEM.

Para utilizar ofertas de fragmentos de experiencia de AEM con el fin de potenciar sus actividades de personalización, pasemos al capítulo siguiente e integremos AEM con Adobe Target mediante los servicios de nube heredados. Esta integración es necesaria para insertar fragmentos de experiencias de AEM a Target como ofertas HTML/JSON y para mantener las ofertas de destino sincronizadas con AEM. Esta integración es necesaria para implementar el [Escenario 1 que se describe en la sección de información general](./overview.md#personalization-using-aem-experience-fragment).

## Requisitos previos

* **AEM**

   * Las instancias de creación y publicación de AEM son necesarias para completar este tutorial. Si todavía no ha configurado la instancia de AEM, puede seguir los pasos [aquí](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud: <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > El cliente debe estar aprovisionado con Experience Platform Launch y Adobe I/O desde [Adobe support](https://helpx.adobe.com/es/contact/enterprise-support.ec.html) o ponerse en contacto con el administrador del sistema



### Integración de AEM con Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Crear un servicio en la nube de Adobe Target mediante la autenticación IMS de Adobe (*Utiliza la API de Adobe Target*) (00:34)
2. Obtener código de cliente de Adobe Target (1:50)
3. Crear la configuración de Adobe IMS para Adobe Target (02:08)
4. Crear una cuenta técnica para acceder a la API de Target en Adobe I/O Console (02:08)
5. Añadir el servicio en la nube de Adobe Target a los fragmentos de experiencia de AEM (04:12)

En este punto, ha integrado correctamente [AEM con Adobe Target mediante los servicios de nube heredados](./using-aem-cloud-services.md#integrating-aem-target-options) como se detalla en la opción 2. Ahora debería poder crear un fragmento de experiencia dentro de AEM y publicar el fragmento de experiencia como oferta HTML u oferta JSON en Adobe Target, y luego utilizarlo para crear una actividad.
