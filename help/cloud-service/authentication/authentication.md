---
title: AEM Autenticación en el as a Cloud Service
description: AEM Obtenga información acerca de la autenticación en los as a Cloud Service de.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 51
source-git-commit: dfb9281abacfe28068b866a8eda786e2d30b9ea6
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 3%

---

# AEM autenticación as a Cloud Service

AEM El as a Cloud Service admite varias opciones de autenticación y varía según el tipo de servicio.

|                       | AEM Author | AEM Publish |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| · [SAML 2.0 a través de Adobe IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html#how-to-set-up) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [Inicio de sesión único (SSO)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [OAuth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html#integration-with-an-idp) | ✘ | ✔ |
| [Autenticación de token](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |
| Autenticación básica | ✘ | ✘ |

## Opciones de autenticación

Haga clic en el vínculo correspondiente a continuación para obtener más información sobre cómo configurar y utilizar el método de autenticación.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          AEM Administre el acceso de Autor de la mediante Adobe IMS a través de Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        AEM Autentique al usuario de su sitio web con un IDP mediante la integración de SAML 2.0 del servicio de publicación de.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Autenticación de token</a></strong></div>
      <p>
        AEM Permitir que las aplicaciones y el middleware se autentiquen para usar un token de servicio de API para la autenticación de la aplicación.
      </p>
    </td>   
  </tr>
</table>
