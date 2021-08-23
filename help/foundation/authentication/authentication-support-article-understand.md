---
title: Comprender la compatibilidad con la autenticación en AEM
description: 'Una vista consolidada de los mecanismos de autenticación (y, ocasionalmente, autorización) admitidos por AEM. '
version: 6.3, 6.4, 6.5
feature: Usuarios y grupos
topics: authentication, security
activity: understand
audience: architect, developer, implementer
doc-type: article
kt: 406
topic: Arquitectura
role: Architect
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 3%

---


# Comprender la compatibilidad con la autenticación en AEM 6.x

Una vista consolidada de los mecanismos de autenticación (y, ocasionalmente, autorización) admitidos por AEM.

*En la tabla siguiente se describe cómo se pueden autenticar los usuarios en AEM.*

<table>
    <tbody>
        <tr>
            <td><strong>Autenticación</strong></td>
            <td><strong>AEM 6.3</strong></td>
            <td><strong>AEM 6.4</strong></td>
            <td><strong>AEM 6.5</strong></td>
        </tr>
        <tr>
            <td><strong>AEM como proveedor de identidad canónico</strong></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td>Autenticación básica</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td>Basado en Forms</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td>Basado en tokens (con <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html" target="_blank">token encapsulado</a>)</td>
            <td>š</td>
            <td>š</td>
            <td>š</td>
        </tr>
        <tr>
            <td><strong>Sistema no AEM como proveedor de identidad canónico</strong></td>
            <td></td>
            <td></td>
            <td></td>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html" target="_blank">LDAP</a></td>
                <td>š</td>
                <td>š</td>
                <td>š</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/single-sign-on.html" target="_blank">SSO</a></td>
                <td>š</td>
                <td>š</td>
                <td>š</td>
            </tr>
            <tr>
                <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/saml-2-0-authenticationhandler.html" target="_blank">SAML 2.0</a></td>
                <td>š</td>
                <td>š</td>
                <td>š</td>
            </tr>
            <tr>
                <td><a href="https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html" target="_blank">OAuth 1.0a y 2.0</a></td>
                <td>š</td>
                <td>š</td>
                <td>š</td>
            </tr>
            <tr>
                <td><a href="https://sling.apache.org/documentation/the-sling-engine/authentication/authentication-authenticationhandler/openid-authenticationhandler.html" target="_blank">OpenID</a></td>
                <td>⁕</td>
                <td>⁕</td>
                <td>*</td>
            </tr>
    </tbody>
</table>

⁕ *Se proporciona a través de proyectos de la comunidad, pero no directamente compatible con Adobe.*
