---
title: AEM Notificación de seguridad de la (noviembre de 2018)
seo-title: AEM Security Notification (November 2018)
description: AEM Dispatcher de notificación de seguridad para Experience Manager
seo-description: AEM Experience Manager Security Notification Dispatcher
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
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 14%

---

# AEM Notificación de seguridad de la (noviembre de 2018)

## Resumen

AEM Este artículo aborda algunas vulnerabilidades recientes y antiguas de las que se ha informado recientemente en la publicación de informes de la organización de informes de seguridad de la comunidad de. AEM Tenga en cuenta que la mayoría de las vulnerabilidades identificadas eran problemas conocidos para el producto y que la mitigación se ha identificado anteriormente, por lo que hay una nueva versión de Dispatcher disponible para las nuevas vulnerabilidades. El Adobe también insta a los clientes a completar la [AEM Lista de comprobación de seguridad](https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/security-checklist.html) y seguir las directrices pertinentes.

## Acción necesaria

* AEM Las implementaciones de deberían empezar a utilizar la versión más reciente de Dispatcher.
* Las reglas de seguridad de Dispatcher deben aplicarse según la configuración recomendada.
* El [AEM Lista de comprobación de seguridad](https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/security-checklist.html) AEM Debe completarse para implementaciones de.

## Vulnerabilidades y soluciones

| Problema | Resolución | Vínculos |
|-------|------------|-------|
| AEM Omitir reglas de Dispatcher de | Instale la última versión de Dispatcher (4.3.1) y siga la configuración recomendada de Dispatcher. | Consulte [AEM Notas de la versión de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) y [Configurar Dispatcher](https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidad de omisión del filtro de URL que podría utilizarse para eludir las reglas de Dispatcher - CVE-2016-0957 | Esto se corrigió en una versión anterior de Dispatcher, pero ahora se recomienda instalar la versión más reciente de Dispatcher (4.3.1) y seguir la configuración recomendada de Dispatcher. | Consulte [AEM Notas de la versión de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) y [Configurar Dispatcher](https://helpx.adobe.com/es/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidad XSS relacionada con archivos de SWF almacenados | Esto se ha solucionado con correcciones de seguridad publicadas anteriormente. | Consulte lo siguiente [AEM Boletín de Seguridad APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Ataques relacionados con contraseñas | Siga las recomendaciones de la Lista de comprobación de seguridad para contraseñas más seguras. | Consulte [AEM Lista de comprobación de seguridad](https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Exposición del uso del disco para usuarios anónimos | AEM AEM Este problema se ha resuelto para la versión 6.1 y posteriores, y para la versión 6.0 se pueden modificar los permisos predeterminados para que sean más restrictivos. | Consulte [notas de la versión](https://helpx.adobe.com/es/experience-manager/aem-previous-versions.html)AEM para 6.1 y versiones posteriores de la. |
| Exposición de Open Social Proxy para usuarios anónimos | Esto se ha resuelto en versiones a partir de 6.0 SP2. | Consulte [notas de la versión](https://helpx.adobe.com/es/experience-manager/aem-previous-versions.html) AEM para 6.1 y versiones posteriores de la. |
| Acceso al Explorador CRX en instancias de producción | La administración del acceso al Explorador de CRX ya está cubierta en la Lista de comprobación de seguridad, el Explorador CRX debe eliminarse del autor y la publicación de producción y la comprobación de estado de seguridad informa de ello si no se elimina. | Consulte [AEM Lista de comprobación de seguridad](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security-checklist.html?lang=es). |
| BGServlets está expuesto | AEM Esto se ha resuelto desde la versión 6.2 de la. | Consulte [AEM Notas de la versión de 6.2](https://helpx.adobe.com/es/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [AEM Guía del usuario de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Notas de la versión de Dispatcher de AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [AEM Boletines de seguridad](https://helpx.adobe.com/security.html#experience-manager)

