---
title: Explicación de la autenticación IMS de Adobe con AEM en los servicios gestionados de Adobe
description: Adobe Experience Manager incorpora compatibilidad con Admin Console para instancias de AEM y autenticación basada en Adobe IMS (Identity Management System) para AEM en Managed Services.   Esta integración permite a AEM clientes de Managed Services administrar todos los usuarios de Experience Cloud en una sola consola web unificada. Los usuarios y los grupos pueden asignarse a perfiles de productos asociados con instancias de AEM, lo que permite el acceso administrado centralmente a instancias de AEM específicas.
version: 6.4, 6.5
feature: authentication
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---


# Explicación de la autenticación IMS de Adobe con AEM en los servicios administrados de Adobe{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager incorpora compatibilidad con Admin Console para instancias de AEM y autenticación basada en Adobe IMS (Identity Management System) para AEM en Managed Services.   Esta integración permite a AEM clientes de Managed Services administrar todos los usuarios de Experience Cloud en una sola consola web unificada. Los usuarios y grupos se pueden asignar a perfiles de producto asociados con instancias de AEM, lo que permite el acceso administrado centralmente a instancias de AEM específicas.

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* La compatibilidad con la autenticación IMS de Adobe Experience Manager solo está disponible para usuarios &quot;internos&quot; (autores, revisores, administradores, desarrolladores, etc.) y no para usuarios finales externos como visitantes de sitios web.
* [Admin ](https://adminconsole.adobe.com/) Consolerepresentará a los clientes de AEM Managed Services como organizaciones IMS y a las instancias de AEM como Contextos de producto. Los administradores de sistemas y productos de Admin Console pueden definir y administrar.
* AEM Managed Services sincroniza su topología con Admin Console, creando una asignación de 1 a 1 entre un contexto de producto y una instancia de AEM.
* El Perfil del producto en Admin Console determinará a qué instancias de AEM puede acceder un usuario.
* La compatibilidad con la autenticación incluye los desplazados internos compatibles con SAML2 del cliente para SSO.
* Solo se admitirán Enterprise ID o Federated ID (para SSO del cliente) (no se admiten los ID de Adobe personal).

** Esta función es compatible con AEM 6.4 SP3 y posterior para los clientes de los servicios gestionados de Adobe.*

## Prácticas recomendadas {#best-practices}

### Aplicación de permisos en Admin Console

La aplicación de permisos y el acceso a nivel de usuario deben evitarse tanto en el Admin Console como en Adobe Experience Manager.

En el Admin Console, se debe otorgar acceso a los usuarios mediante grupos de usuarios en el nivel de contexto de producto. Los grupos de usuarios suelen expresarse mejor por función lógica dentro de la organización para promover la reutilización de los grupos en todos los productos de Adobe Experience Cloud.

>[!NOTE]
>
> Si utiliza AEM como Cloud Service, asigne usuarios Admin Console directamente a los Perfiles de producto. Los permisos transitivos entre usuarios Admin Console y Perfiles de producto a través de grupos de usuarios Admin Console no son compatibles para AEM como Cloud Service.

### Aplicación de permisos en Adobe Experience Manager

En Adobe Experience Manager, los grupos de usuarios sincronizados con IMS de Adobe deben agregarse a término a [grupos de usuarios proporcionados por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html), que vienen preconfigurados con los permisos adecuados para ejecutar conjuntos específicos de tareas en AEM. Los usuarios sincronizados con IMS de Adobe no deben agregarse directamente a [grupos de usuarios proporcionados por AEM](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html).
