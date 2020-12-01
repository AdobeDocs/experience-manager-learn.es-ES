---
title: Integración de Adobe Experience Manager con Adobe Target mediante Cloud Services
seo-title: Integración de Adobe Experience Manager (AEM) con Adobe Target mediante Cloud Services heredados
description: Recorrido paso a paso sobre cómo integrar Adobe Experience Manager (AEM) con Adobe Target mediante AEM Cloud Service
seo-description: Recorrido paso a paso sobre cómo integrar Adobe Experience Manager (AEM) con Adobe Target mediante AEM Cloud Service
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---


# Uso de AEM Cloud Services heredados

En esta sección analizaremos cómo configurar Adobe Experience Manager (AEM) con Adobe Target mediante Cloud Services heredados.

>[!NOTE]
>
> El Cloud Service heredado AEM con Adobe Target se **sólo** utiliza para establecer una conexión directa de AEM Author a Adobe Target back-end, lo que facilita la publicación de contenido de AEM a Destinatario. Se utiliza Inicio de Adobe para exponer a Adobe Target en la experiencia del sitio Web de cara al público que ofrece AEM.

Para utilizar AEM ofertas de fragmentos de experiencia con el fin de ayudarle a crear actividades de personalización, continúe con el capítulo siguiente e integre AEM con Adobe Target mediante los servicios de nube heredados. Esta integración es necesaria para insertar fragmentos de experiencia de AEM a Destinatario como ofertas HTML/JSON y mantener las ofertas de destinatario sincronizadas con AEM. Esta integración es necesaria para implementar el [Escenario 1 analizado en la sección de información general](./overview.md#personalization-using-aem-experience-fragment).

## Requisitos previos

* **AEM**

   * AEM instancia de creación y publicación son necesarias para completar este tutorial. Si todavía no ha configurado la instancia de AEM, puede seguir los pasos [aquí](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Acceso a sus organizaciones Adobe Experience Cloud - <https://>`<yourcompany>`.experienceCloud.adobe.com
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > El cliente debe estar aprovisionado con Experience Platform Launch y Adobe I/O desde [Adobe support](https://helpx.adobe.com/es/contact/enterprise-support.ec.html) o ponerse en contacto con el administrador del sistema



### Integración de AEM con Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Crear un Cloud Service de Adobe Target mediante la autenticación de Adobe IMS (*Utiliza Adobe Target API*) (00:34)
2. Obtención del código de cliente de Adobe Target (01:50)
3. Creación de la configuración de IMS de Adobe para Adobe Target (02:08)
4. Crear una cuenta técnica para acceder a la API de Destinatario en la consola de Adobe I/O (02:08)
5. Añadir Cloud Service de Adobe Target a fragmentos de experiencia AEM (04:12)

En este punto, ha integrado correctamente [AEM con Adobe Target mediante Cloud Services heredados](./using-aem-cloud-services.md#integrating-aem-target-options) como se detalla en la opción 2. Ahora debe poder crear un fragmento de experiencia dentro de AEM, publicar el fragmento de experiencia como oferta HTML o Oferta JSON en Adobe Target y, a continuación, puede utilizarse para crear una actividad.
