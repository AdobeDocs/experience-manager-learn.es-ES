---
title: AEM Explicación de la autenticación IMS de Adobe con el uso de en Adobe Managed Services
description: Adobe Experience Manager presenta la compatibilidad de Admin Console AEM para instancias de y la autenticación basada en Adobe IMS (Identity Management AEM System) para la autenticación en Managed Services.   AEM Esta integración permite a los clientes de Managed Services de los usuarios de la aplicación administrar todos los usuarios de Experience Cloud en una única consola web unificada. AEM AEM Los usuarios y grupos pueden asignarse a perfiles de producto asociados a instancias de, lo que otorga acceso administrado de forma centralizada a las instancias específicas de la instancia de la aplicación.
version: 6.4, 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 405
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---

# AEM Explicación de la autenticación IMS de Adobe con el uso de en Adobe Managed Services{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager introduce la compatibilidad del Admin Console AEM con instancias de y la autenticación basada en Identity Management AEM System (IMS) de Adobe para la autenticación de en Managed Services.   AEM Esta integración permite a los clientes de Managed Services de los usuarios de la aplicación administrar todos los usuarios de Experience Cloud en una única consola web unificada. AEM AEM Los usuarios y grupos se pueden asignar a perfiles de producto asociados a instancias de, lo que otorga acceso administrado centralmente a las instancias de usuario específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La compatibilidad con la autenticación IMS de Adobe Experience Manager solo es para usuarios &quot;internos&quot; (autores, revisores, administradores, desarrolladores, etc.), y no para usuarios finales externos como visitantes de sitios web.
* [Admin Console](https://adminconsole.adobe.com/) AEM representa a clientes de Managed Services AEM como organizaciones de IMS y las instancias de como contextos de producto. Los administradores de sistemas y productos Admin Console pueden definir y administrar.
* AEM Managed Services sincronizar su topología con Admin Console AEM, creando una asignación de 1 a 1 entre un contexto de producto y una instancia de.
* El perfil de producto en el Admin Console AEM determina a qué instancias de puede acceder un usuario.
* El soporte de autenticación incluye IDPs compatibles con SAML2 para SSO.
* Solo se admiten Enterprise ID o Federated ID (para el SSO del cliente) (no se admiten los ID de Adobe personal).

*&#42;AEM Esta función es compatible con el SP3 de la versión 6.4 y versiones posteriores para los clientes de Adobe Managed Services.*

## Prácticas recomendadas {#best-practices}

### Aplicación de permisos en Admin Console

La aplicación de permisos y acceso en el nivel de usuario debe evitarse tanto en Admin Console como en Adobe Experience Manager.

En Admin Console, los usuarios deben tener acceso a través de grupos de usuarios en el nivel de contexto de producto. Los grupos de usuarios suelen expresarse mejor por la función lógica dentro de la organización para promover la reutilización de los grupos en los productos de Adobe Experience Cloud.

### Aplicación de permisos en Adobe Experience Manager

En Adobe Experience Manager, los grupos de usuarios sincronizados desde Adobe IMS deben agregarse a [AEM Grupos de usuarios proporcionados por el usuario](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=es)AEM , que vienen preconfiguradas con los permisos adecuados para ejecutar conjuntos específicos de tareas en el entorno de trabajo de la aplicación de la configuración de la. Los usuarios sincronizados desde Adobe IMS no deben añadirse directamente a [AEM Grupos de usuarios proporcionados por el usuario](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=es).
