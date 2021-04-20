---
title: Notificación de seguridad de AEM (noviembre de 2018)
seo-title: Notificación de seguridad de AEM (noviembre de 2018)
description: Dispatcher de notificaciones de seguridad de AEM Experience Manager
seo-description: Dispatcher de notificaciones de seguridad de AEM Experience Manager
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 8%

---


# Notificación de seguridad de AEM (noviembre de 2018)

## Resumen

Este artículo aborda algunas vulnerabilidades recientes y antiguas que se han registrado recientemente en AEM. Tenga en cuenta que la mayoría de las vulnerabilidades identificadas eran problemas conocidos para el producto AEM y que la mitigación se ha identificado anteriormente, y hay una nueva versión de Dispatcher disponible para las nuevas vulnerabilidades. Adobe también insta a los clientes a completar la [lista de comprobación de seguridad de AEM](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) y seguir las directrices relevantes.

## Acción necesaria

* Las implementaciones de AEM deben comenzar a utilizar la última versión de Dispatcher.
* Las reglas de seguridad de Dispatcher deben aplicarse según la configuración recomendada.
* La [AEM Security Checklist](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) debe completarse para implementaciones de AEM.

## Vulnerabilidades y resoluciones

| Problema | Resolución | Vínculos |
|-------|------------|-------|
| Omisión de las reglas de Dispatcher de AEM | Instale la versión más reciente de Dispatcher(4.3.1) y siga la configuración recomendada de Dispatcher. | Consulte [Notas de la versión de Dispatcher de AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) y [Configuración de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidad del filtro de URL que se podría usar para eludir las reglas de Dispatcher - CVE-2016-0957 | Se corrigió en una versión anterior de Dispatcher, pero ahora se recomienda instalar la versión más reciente de Dispatcher (4.3.1) y seguir la configuración recomendada de Dispatcher. | Consulte [Notas de la versión de Dispatcher de AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) y [Configuración de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidad XSS relacionada con archivos SWF almacenados | Esto se ha solucionado con correcciones de seguridad publicadas anteriormente. | Consulte el [Boletín de seguridad de AEM APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Explosiones relacionadas con contraseña | Siga la recomendación de la lista de comprobación de seguridad para obtener contraseñas más seguras. | Consulte [AEM Security Checklist](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Exposición del uso del disco para usuarios anónimos | Este problema se ha resuelto para AEM 6.1 y posteriores, para AEM 6.0 los permisos predeterminados se pueden modificar para que sean más restrictivos. | Consulte las [notas de la versión](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=es#previous-updates)para AEM 6.1 y versiones anteriores. |
| Exposición del proxy abierto de Social para usuarios anónimos | Esto se ha resuelto en versiones que comienzan desde 6.0 SP2. | Consulte las [notas de la versión](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) para AEM 6.1 y versiones anteriores. |
| Acceso al explorador CRX en instancias de producción | La administración del acceso al Explorador CRX ya está cubierta en la lista de comprobación de seguridad, el Explorador CRX debe eliminarse del autor y la publicación de producción y la comprobación de estado de seguridad lo informa si no se elimina. | Consulte [AEM Security Checklist](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html). |
| BGServlets está expuesto | Esto se ha resuelto desde AEM 6.2. | Consulte [Notas de la versión de AEM 6.2](https://helpx.adobe.com/es/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guía del usuario de AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Notas de la versión de Dispatcher de AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Boletines de seguridad de AEM](https://helpx.adobe.com/security.html#experience-manager)

