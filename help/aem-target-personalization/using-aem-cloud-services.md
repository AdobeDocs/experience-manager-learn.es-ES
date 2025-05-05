---
title: Integración de Adobe Experience Manager con Adobe Target mediante Cloud Service
description: Tutorial paso a paso sobre cómo integrar Adobe Experience Manager AEM () con Adobe Target mediante AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 337
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 1%

---

# AEM Uso de Cloud Service heredados de

En esta sección, analizaremos cómo configurar Adobe Experience Manager AEM () con Adobe Target mediante Cloud Service heredados.

>[!NOTE]
>
> AEM El Cloud Service de legado de la con Adobe Target AEM es **only** y se utiliza para establecer una conexión directa entre el autor y el back-end de Adobe Target AEM que facilita la publicación de contenido desde el sitio de trabajo de la a Target. Las etiquetas en Adobe Experience Platform se utilizan para exponer Adobe Target AEM en la experiencia pública del sitio web atendida por los usuarios de la red de servicios de la red de servicios de la red de.

AEM AEM Para utilizar ofertas de fragmentos de experiencias de para potenciar sus actividades de personalización, continúe con el siguiente capítulo e integre la experiencia con Adobe Target mediante los servicios en la nube heredados. AEM Esta integración es necesaria para insertar los fragmentos de experiencias de Target en Target como ofertas de HTML AEM/JSON y para mantener las ofertas de Target sincronizadas con las ofertas de. Esta integración es necesaria para implementar [el escenario 1 descrito en la sección de información general](./overview.md#personalization-using-aem-experience-fragment).

## Requisitos previos

* AEM **&#x200B;**

   * AEM Para completar este tutorial se necesita una instancia de autor y publicación de. AEM Si aún no ha configurado la instancia de la instancia de la, puede seguir los pasos [aquí](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Acceso a Adobe Experience Cloud de sus organizaciones - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud aprovisionado con las siguientes soluciones
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > Es necesario que el cliente reciba la recopilación de datos y el Adobe I/O de [Soporte técnico para Adobes](https://helpx.adobe.com/es/contact/enterprise-support.ec.html) o que se ponga en contacto con el administrador del sistema

### AEM Integración de los usuarios de con Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Crear Cloud Service de Adobe Target mediante la autenticación IMS de Adobe (*Utiliza la API de Adobe Target*) (00:34)
2. Obtener el código de cliente de Adobe Target (1:50)
3. Creación de la configuración de IMS de Adobe para Adobe Target (2:08)
4. Cree una cuenta técnica para acceder a la API de Target en Adobe I/O Console (02:08)
5. Adición del Cloud Service AEM de Adobe Target a los fragmentos de experiencias de la (04:12)

AEM En este punto, ha integrado correctamente [con Adobe Target mediante Cloud Service heredados](./using-aem-cloud-services.md#integrating-aem-target-options), tal como se detalla en la opción 2. AEM Ahora debería poder crear un fragmento de experiencia dentro de, y publicar el fragmento de experiencia como oferta de HTML u oferta JSON en Adobe Target, y luego se puede usar para crear una actividad.
