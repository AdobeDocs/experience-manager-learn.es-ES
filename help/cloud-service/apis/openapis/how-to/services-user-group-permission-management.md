---
title: Administración de permisos del grupo de usuarios Perfil de productos y servicios
description: Obtenga información sobre cómo administrar permisos para los grupos de usuarios Perfil de productos y Servicios en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17429
thumbnail: KT-17429.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 3230a8e7-6342-4497-9163-1898700f29a4
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# Administración de permisos del grupo de usuarios Perfil de productos y servicios

Obtenga información sobre cómo administrar permisos para los grupos de usuarios Perfil de productos y Servicios en AEM as a Cloud Service.

En este tutorial, aprenderá lo siguiente:

- Perfil del producto y su asociación con los servicios.
- Actualizar los permisos del grupo de usuarios de servicios.

## Información general

Cuando usa una API de AEM, debe asignar el _perfil de producto_ a las _credenciales_ en el proyecto de Adobe Developer Console (o ADC). El _Perfil de producto_ (y el servicio asociado) proporciona los _permisos o autorización_ para las credenciales de acceso a los recursos de AEM. En la siguiente captura de pantalla, puede ver las _credenciales_ y el _perfil de producto_ para una API de autor de AEM Assets:

![Credenciales y perfil de producto](../assets/how-to/API-Credentials-Product-Profile.png)

Un perfil de producto está asociado con uno o más _servicios_. En AEM as a Cloud Service, un _servicio_ representa grupos de usuarios con Listas de control de acceso (ACL) predefinidas para los nodos del repositorio, lo que permite la administración granular de permisos.

![Perfil de producto de usuario de cuenta técnica](../assets/s2s/technical-account-user-product-profile.png)

Una vez invocada correctamente la API, se crea un usuario que representa las credenciales del proyecto ADC en el servicio de creación de AEM, junto con los grupos de usuarios que coinciden con la configuración del perfil de producto y los servicios.

![Pertenencia de usuario de cuenta técnica](../assets/s2s/technical-account-user-membership.png)

En el escenario anterior, se crea el usuario `1323d2...` en el servicio de creación de AEM y es miembro de los grupos de usuarios `AEM Assets Collaborator Users - Service` y `AEM Assets Collaborator Users - author - Program XXX - Environment XXX`.

## Permisos del grupo de usuarios de Update Services

La mayoría de los _servicios_ proporcionan el permiso _READ_ a los recursos de AEM a través de los grupos de usuarios en la instancia de AEM que tienen el mismo nombre que _Servicio_.

Hay ocasiones en que las credenciales (también conocido como usuario de cuenta técnica) necesitan permisos adicionales como _Crear, Actualizar, Eliminar_ (CUD) de recursos de AEM. En estos casos, puede actualizar los permisos de los grupos de usuarios _Services_ en la instancia de AEM.

Por ejemplo, cuando la invocación de la API de autor de AEM Assets recibe un error [403 en solicitudes que no son de GET](../use-cases/invoke-api-using-oauth-s2s.md#403-error-for-non-get-requests), puede actualizar los permisos del grupo de usuarios _Usuarios de AEM Assets Collaborator: servicio_ en la instancia de AEM.

Con la interfaz de usuario de permisos o el script [Inicialización del repositorio de Sling](https://sling.apache.org/documentation/bundles/repository-initialization.html), puede actualizar los permisos de los grupos de usuarios predeterminados en la instancia de AEM.

### Actualizar permisos mediante la interfaz de usuario de permisos

Para actualizar los permisos del grupo de usuarios de servicios (por ejemplo, `AEM Assets Collaborator Users - Service`) mediante la interfaz de usuario de permisos, siga estos pasos:

- Vaya a **Herramientas** > **Seguridad** > **Permisos** en la instancia de AEM.

- Busque el grupo de usuarios de servicios (por ejemplo `AEM Assets Collaborator Users - Service`).

  ![Buscar grupo de usuarios](../assets/how-to/search-user-group.png)

- Haga clic en **Agregar ACE** para agregar una nueva Entrada de control de acceso (ACE) para el grupo de usuarios.

  ![Agregar ACE](../assets/how-to/add-ace.png)

### Actualizar permisos mediante el repositorio Script de inicialización

Para actualizar los permisos del grupo de usuarios de servicios (por ejemplo, `AEM Assets Collaborator Users - Service`) mediante el script de inicialización del repositorio, siga estos pasos:

- Abra el proyecto de AEM en su IDE favorito.

- Vaya al módulo `ui.config`

- Cree un archivo en `ui.config/src/main/content/jcr_root/apps/<PROJECT-NAME>/osgiconfig/config.author` con el nombre `org.apache.sling.jcr.repoinit.RepositoryInitializer-services-group-acl-update.cfg.json`, con el siguiente contenido:

  ```json
  {
      "scripts": [
          "set ACL for \"AEM Assets Collaborator Users - Service\" (ACLOptions=ignoreMissingPrincipal)",
          "    allow jcr:read,jcr:versionManagement,crx:replicate,rep:write on /content/dam",
          "end"
      ]
  }
  ```

- Confirme y envíe los cambios al repositorio.

- Implemente los cambios en la instancia de AEM mediante la [canalización de pila completa de Cloud Manager](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#full-stack-pipeline).

- También puede comprobar los permisos del grupo de usuarios mediante la vista **Permisos**. Vaya a **Herramientas** > **Seguridad** > **Permisos** en la instancia de AEM.

  ![Vista de permisos](../assets/how-to/permissions-view.png)

### Compruebe los permisos

Después de actualizar los permisos mediante cualquiera de los métodos anteriores, la solicitud de PATCH para actualizar los metadatos del recurso debería funcionar ahora sin problemas.

![Solicitud de PATCH](../assets/how-to/patch-request.png)

## Resumen

Ha aprendido a administrar permisos para los grupos de usuarios Perfil de productos y Servicios en AEM as a Cloud Service. Puede actualizar los permisos de los grupos de usuarios de servicios en la instancia de AEM mediante la interfaz de usuario de permisos o el script de inicialización del repositorio.
