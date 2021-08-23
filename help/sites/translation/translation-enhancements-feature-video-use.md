---
title: Mejoras de traducción en AEM
description: AEM marco de trabajo de traducción robusto permite que AEM contenido se traduzca sin problemas por proveedores de traducción admitidos. Obtenga más información sobre las últimas mejoras.
version: 6.3, 6.4, 6.5
topic: Localización
feature: Administrador de varios sitios, copia de idioma
role: User
level: Beginner
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 2%

---


# Mejoras de traducción con Multi-Site Manager {#translation-enhancements}

AEM marco de trabajo de traducción robusto permite que AEM contenido se traduzca sin problemas por proveedores de traducción admitidos.

## Mejoras de traducción en AEM 6.5

>[!VIDEO](https://video.tv.adobe.com/v/27405?quality=9&learn=on)

AEM mejoras en la traducción de la versión 6.5 incluyen:

**Aprobación automática de trabajos** de traducción: El indicador de aprobación en el trabajo de traducción es una propiedad binaria. No conduce ni integra con los flujos de trabajo de revisión y aprobación listos para usar. Para que el número de pasos de un trabajo de traducción sea mínimo, de forma predeterminada se establece en &quot;aprobar automáticamente&quot; en [!UICONTROL Propiedades avanzadas] de un proyecto de traducción. Si su organización requiere aprobación para un trabajo de traducción, puede desactivar la opción &quot;aprobar automáticamente&quot; en [!UICONTROL Propiedades avanzadas] de un proyecto de traducción.

**Eliminar automáticamente los lanzamientos** de traducción: En lugar de eliminar manualmente los lanzamientos de traducción en el administrador de lanzamientos después del hecho, ahora es posible eliminar automáticamente los lanzamientos de traducción después de promoverlos.

**Exportar objetos de traducción en formato** JSON: AEM 6.4 y versiones anteriores admiten los formatos XML y XLIFF de objetos de traducción. Ahora puede configurar el formato de exportación al formato JSON mediante la consola de sistemas [!UICONTROL Administrador de configuración]. Busque [!UICONTROL Translation Platform Configuration] y, a continuación, puede seleccionar el formato de exportación como JSON.

**Actualice el contenido AEM traducido en la memoria de traducción (TMS)**: el autor local que no tiene acceso a AEM puede realizar actualizaciones en el contenido traducido, que ya se haya vuelto a introducir en AEM, directamente en la TM (Memoria de traducción, en el sistema de administración de etiquetas), y actualizar las traducciones en AEM reenviando el trabajo de traducción de los sistemas de administración de etiquetas a AEM

## Mejoras de traducción en AEM 6.4

>[!VIDEO](https://video.tv.adobe.com/v/21309?quality=9&learn=on)

Los autores ahora pueden crear rápida y fácilmente proyectos de traducción en varios idiomas directamente desde el administrador de sitios o el administrador de proyectos, configurar dichos proyectos para promocionar automáticamente los lanzamientos e incluso establecer programaciones para la automatización.

## Recursos adicionales {#additional-resources}

* [Traducción de contenido para sitios multilingües](https://helpx.adobe.com/es/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Prácticas recomendadas de traducción](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)