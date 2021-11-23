---
title: Incorporación a AEM as a Cloud Service
description: Obtenga información sobre la incorporación a AEM as a Cloud Service, empezando por la fase de contrato hasta la configuración de entornos mediante Cloud Manager.
version: Cloud Service
feature: Onboarding
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8631
thumbnail: 336959.jpeg
exl-id: 9d2004e5-e928-4190-8298-695635c8e92c
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 6%

---

# Incorporación a AEM as a Cloud Service

Obtenga información sobre la incorporación a AEM as a Cloud Service desde la fase de contrato hasta la configuración de entornos mediante Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/336959/?quality=12&learn=on)

## Cloud Manager y Admin Console

![Diagrama de alto nivel de integración](assets/onboarding-diagram.png)

Una parte fundamental de la incorporación es crear AEM programas as a Cloud Service y aprovisionar varios entornos mediante Adobe Cloud Manager. La variable [Admin Console](https://adminconsole.adobe.com/) se utiliza para asignar funciones y proporcionar acceso a los usuarios de la organización a entornos AEM.

## Actividades clave

+ Un administrador del sistema utiliza la variable [Admin Console](https://adminconsole.adobe.com/) para asignar uno o varios usuarios a la variable [Cloud Manager: propietario empresarial](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html) perfil de producto.
+ Los usuarios asignados al Perfil de producto del propietario empresarial utilizan las características de autoservicio de [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=es) a [crear programas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/production-programs/creating-production-program.html) y [añadir entornos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html)
+ Utilice la variable [Admin Console](https://adminconsole.adobe.com/) para asignar desarrolladores y usuarios a diferentes [Funciones de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html) y conceder permiso a varios entornos de AEM.

## Ejercicio práctico

Aplique sus conocimientos probando lo que ha aprendido con este ejercicio práctico.

Antes de intentar el ejercicio práctico, asegúrese de haber visto y comprendido el vídeo de arriba y los siguientes materiales:

+ [Pensar de forma diferente en AEM as a Cloud Service](./introduction.md)
+ [Cloud Manager](./cloud-manager.md)

Además, asegúrese de haber completado el ejercicio práctico anterior:

+ [Ejercicio práctico AEM Herramientas de modernización](./aem-modernization-tools.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session3-onboarding#bootcamp---session-3-on-boarding"><img alt="Repositorio de GitHub de ejercicios prácticos" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Manos con incorporación</div>
            <p style="margin:1rem 0">
                Explore AEM proceso de incorporación as a Cloud Service y cómo implementar una aplicación AEM en el SDK de AEM.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session3-onboarding#bootcamp---session-3-on-boarding" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Pruebe a incorporar</span>
            </a>
        </td>
    </tr>
</table>
