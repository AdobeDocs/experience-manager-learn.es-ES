---
title: Cree una cuenta de Adobe Target Cloud Service en AEM
description: Integre Adobe Experience Manager as a Cloud Service con Adobe Target mediante la autenticación IMS de Cloud Service y Adobe.
jira: KT-6044
thumbnail: 41244.jpg
topic: Integrations
feature: Integrations
role: Admin
level: Intermediate
badgeIntegration: label="Integración" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: dd6c17ae-8e08-4db3-95f9-081cc7dbd86e
duration: 316
source-git-commit: 22bd6237bb1665bf87c6302d2988135b505e40c0
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%

---

# Crear cuenta de Adobe Target Cloud Service {#adobe-target-cloud-service}

El siguiente vídeo proporciona información general sobre cómo conectar AEM as a Cloud Service con Adobe Target.

Esta integración permite que el servicio de creación de AEM se comunique directamente con Adobe Target y envíe los fragmentos de experiencias de AEM a Target como ofertas.  Esta integración hace que *not* agregue Adobe Target JavaScript (AT.js) a las páginas web de AEM Sites, para lo cual integra [AEM y etiquetas con la extensión de Target](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md).

>[!WARNING]
>
>El vídeo muestra el método de autenticación JWT obsoleto para conectar AEM a Adobe Target. Sin embargo, el método recomendado es utilizar el método de autenticación de servidor a servidor OAuth. Para obtener más información, consulte [Migración de credenciales de JWT a OAuth para AEM](https://experienceleague.adobe.com/en/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration.html). Estamos trabajando en la actualización del vídeo para reflejar este cambio.


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Hay un problema conocido con la configuración de Adobe Target Cloud Services que se muestra en el vídeo. Hasta que se resuelva este problema, siga los mismos pasos en el vídeo pero use la [configuración heredada de Adobe Target Cloud Services](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html).
