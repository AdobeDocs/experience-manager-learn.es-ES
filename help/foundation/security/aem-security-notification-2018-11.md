---
title: Notificación de seguridad AEM (noviembre de 2018)
seo-title: Notificación de seguridad AEM (noviembre de 2018)
description: Distribuidor de notificaciones de seguridad de AEM Experience Manager
seo-description: Distribuidor de notificaciones de seguridad de AEM Experience Manager
version: 6.4
feature: dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 5%

---


# Notificación de seguridad AEM (noviembre de 2018)

## Resumen

Este artículo aborda algunas vulnerabilidades recientes y antiguas que se han informado recientemente en AEM. Tenga en cuenta que la mayoría de las vulnerabilidades identificadas eran problemas conocidos para el producto AEM y que la mitigación se ha identificado anteriormente, y que existe una nueva versión de despachante para las nuevas vulnerabilidades. Adobe también insta a los clientes a completar la lista [de comprobación de seguridad de](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) AEM y seguir las directrices pertinentes.

## Acción necesaria

* Las implementaciones de AEM deben inicio con la última versión de Dispatcher.
* Las reglas de seguridad del despachante deben aplicarse según la configuración recomendada.
* La [AEM lista](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) de comprobación de seguridad debe completarse para AEM implementaciones.

## Vulnerabilidades y resoluciones

| Problema | Resolución | Vínculos |
|-------|------------|-------|
| Omisión de las reglas de AEM Dispatcher | Instale la versión más reciente de Dispatcher(4.3.1) y siga la configuración recomendada del despachante. | Consulte [AEM Notas](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) de revisión de Dispatcher y [Configuración de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidad de evasión de filtro de URL que se puede utilizar para eludir las reglas de despachante: CVE-2016-0957 | Esto se ha corregido en una versión anterior de Dispatcher, pero ahora se recomienda instalar la versión más reciente de Dispatcher (4.3.1) y seguir la configuración recomendada de Dispatcher. | Consulte [AEM Notas](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) de revisión de Dispatcher y [Configuración de Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidad XSS relacionada con archivos SWF almacenados | Esto se ha solucionado con correcciones de seguridad publicadas anteriormente. | Consulte [AEM Boletín de seguridad APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Explosiones relacionadas con la contraseña | Siga las recomendaciones de la lista de verificación Seguridad para obtener contraseñas más sólidas. | Consulte [AEM lista de comprobación de seguridad](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Exposición de uso del disco para usuarios anónimos | Este problema se ha resuelto para AEM 6.1 y posterior, para AEM 6.0 los permisos predeterminados se pueden modificar para que sean más restrictivos. | Consulte las notas [de la](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)versión de AEM 6.1 y versiones anteriores. |
| Exposición del proxy social abierto para usuarios anónimos | Esto se ha resuelto en versiones que comienzan desde 6.0 SP2. | Consulte las notas [de la versión](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) de AEM 6.1 y versiones anteriores. |
| Acceso al Explorador de CRX en instancias de producción | La administración del acceso al Explorador de CRX ya está cubierta en la lista de comprobación de seguridad, el Explorador de CRX debe eliminarse del autor y la publicación de producción y la comprobación de estado de seguridad lo notificará si no se elimina. | Consulte [AEM Lista](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security-checklist.html)de comprobación de seguridad. |
| BGServlets está expuesto | Esto se ha resuelto desde AEM 6.2. | See [AEM 6.2 Release Notes](https://helpx.adobe.com/es/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guía del usuario de AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Notas de la versión de Dispatcher de AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Boletines de seguridad de AEM](https://helpx.adobe.com/security.html#experience-manager)

