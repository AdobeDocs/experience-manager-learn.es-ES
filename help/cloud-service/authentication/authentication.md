---
title: Autenticación en AEM as a Cloud Service
description: Obtenga información sobre la autenticación en AEM de as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 10436
thumbnail: KT-10436.jpeg
source-git-commit: e666e38d6b2a7057f7016b35ad1034a4487e9bc7
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 4%

---


# AEM autenticación as a Cloud Service

AEM as a Cloud Service admite varias opciones de autenticación y varía según el tipo de servicio.

|  | AEM Author | AEM Publish |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | š | ü |
| [SAML 2.0](./saml-2-0.md) | ü | š |
| [Autenticación de tokens](../../headless-tutorial/authentication/overview.md) | š | š |

## Opciones de autenticación

Haga clic en el vínculo correspondiente a para obtener más información sobre cómo configurar y utilizar el método de autenticación.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Administre el acceso de AEM Author mediante Adobe IMS a través de Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Autentíquese el usuario del sitio web a un IDP mediante la integración SAML 2.0 del servicio AEM Publish.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Token" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Autenticación de tokens</a></strong></div>
      <p>
        Permita que las aplicaciones y el middleware se autentiquen en AEM mediante un token de servicio de API.
      </p>
    </td>   
  </tr>
</table>
