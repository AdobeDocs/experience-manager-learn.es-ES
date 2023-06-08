---
title: Guía de mantenimiento rutinario del sitio
seo-title: Your Routine Site Maintenance Guide
description: Tanto si es administrador, autor o desarrollador, el mantenimiento del sitio afecta a todos los aspectos de la instancia de AEM Sites. Utilice esta guía para asegurarse de que la estrategia está configurada para el éxito.
seo-description: Whether you're an admin, author, or developer, site maintenance touches every aspect of your AEM Sites instance. Use this guide to ensure your strategy is set up for success.
audience: author, marketer, developer
feature: Learn From Your Peers
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: 2a137f71cbd876db0164e84ab437e8eda982270e
workflow-type: tm+mt
source-wordcount: '1084'
ht-degree: 5%

---

# Consejos y trucos para el mantenimiento del sitio

AEM Existen tres opciones a la hora de instalar y mantener una instancia de

* AEMaaCS (servicio en la nube): el sistema siempre está encendido, actualizado y se adapta dinámicamente según lo necesite
* Adobe Managed Services, donde los ingenieros de servicio de atención al cliente de Adobe realizan todo el mantenimiento diario, semanal o mensual y se aseguran de que todos los paquetes de servicio estén instalados y de que el sistema esté siempre seguro y funcione sin problemas
* ejecutarlo en línea, donde tiene que encargarse de todo el sistema, incluidas las copias de seguridad, las actualizaciones y la seguridad.

Si decide implementar su propio sistema de forma local, tenga en cuenta lo siguiente para garantizar que dispone de un sistema seguro y eficaz. AEM Además de los artículos de &quot;cuidado y alimentación&quot;, este documento también señalará varios artículos que los desarrolladores deben tener en cuenta para ayudar a mantener el sistema funcionando bien.

## Administradores

Copias de seguridad: asegúrese de que tiene copias de seguridad completas o parciales según una programación frecuente:

* cada día
* cada semana
* mensualmente

Muchos clientes realizan copias de seguridad de instantáneas, que solo tardan unos minutos si el sistema operativo subyacente admite dichas copias de seguridad. AEM Asegúrese de que estas copias de seguridad están almacenadas correctamente (fuera del sistema de). Asegúrese de que las copias de seguridad funcionen y se puedan usar para recrear un sistema en funcionamiento periódicamente. No hay nada peor que el bloqueo del sistema y que las copias de seguridad estén dañadas por alguna razón.

Hay varios elementos que debe monitorizar para garantizar un funcionamiento sin problemas:

### Mantenimiento habitual

#### [mantenimiento de índice](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=es)

Los índices permiten que las consultas se ejecuten lo más rápido posible, lo que libera recursos para otras operaciones. Asegúrese de que los índices estén en forma de extremo superior. AEM AEM cancela las consultas que atraviesan en lugar de utilizar un índice para evitar que una consulta incorrecta afecte al rendimiento general de la.

#### [Compactación Tar/Limpieza de revisión](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Cada actualización del repositorio crea una nueva revisión de contenido. Como resultado, con cada actualización el tamaño del repositorio aumenta. Para evitar el crecimiento incontrolado del repositorio, es necesario limpiar las revisiones antiguas para liberar recursos de disco.

#### [Limpieza de binarios de Lucene](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html#automated-maintenance-tasks)

Purgue los binarios de Lucene y reduzca el requisito de tamaño del almacén de datos en ejecución.

#### [Basura del almacén de datos](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html)

AEM Cuando se elimina un recurso en, la referencia al registro del almacén de datos subyacente se puede eliminar de la jerarquía de nodos, pero el registro del almacén de datos en sí permanece. Este registro de almacén de datos al que no se hace referencia pasa a ser &quot;basura&quot; y no es necesario conservarlo. En los casos en los que existen una serie de recursos sin referencia, es beneficioso deshacerse de ellos para, conservar el espacio, optimizar la copia de seguridad y el rendimiento de mantenimiento del sistema de archivos.

#### [Depuración de flujo de trabajo](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html?lang=es)

Al minimizar el número de instancias de flujo de trabajo, aumenta el rendimiento del motor de flujo de trabajo, por lo que puede depurar con regularidad las instancias de flujo de trabajo completadas o en ejecución desde el repositorio.

#### [Mantenimiento del registro de auditoría](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html

AEM Los eventos de que cumplen los requisitos para el registro de auditoría generan muchos datos archivados. Estos datos pueden crecer rápidamente con el tiempo debido a las replicaciones, las cargas de recursos y otras actividades del sistema.

#### [Seguridad](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=es)

AEM Asegúrese de que se siguen de cerca las prácticas recomendadas de la lista de comprobación de seguridad para garantizar la instancia más segura de los usuarios de la aplicación de seguridad de la.

#### Diskspace

Supervise el espacio en disco para asegurarse de que tiene suficiente espacio para el repositorio JCR, además de aproximadamente la mitad del espacio, la compactación de tar utiliza espacio adicional cuando se ejecuta. ¡Quedarse sin espacio en disco es la razón número uno para la corrupción de JCR!

## Desarrollador

Intente no utilizar componentes personalizados: use [componentes principales](https://www.aemcomponents.dev/). El objetivo debe ser utilizar los componentes principales el 80-90 % de las veces y los componentes personalizados solo con moderación. Esto a menudo requiere una nueva forma de ver los componentes de una página: debe darse cuenta de que un desarrollador front-end que utiliza CSS puede rediseñar fácilmente los componentes. Además, tenga en cuenta que estos componentes principales pueden incrustarse entre sí para lograr resultados bastante complejos. ¡Sé creativo!

### [Sistemas de estilos](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

Los sistemas de estilos permiten que los componentes principales, e incluso los componentes personalizados, cambien de aspecto a discreción de los autores para crear componentes de aspecto completamente nuevos. Estos cambios de estilo generalmente solo implican a un diseñador front-end y a un autor experto (a menudo denominado &quot;superautor&quot;)

### [Lanzamientos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

Los lanzamientos permiten completar el trabajo de una nueva promoción, venta o lanzamiento de un sitio web sin afectar a las páginas implementadas actualmente. Además, se les puede programar para que se publiquen automáticamente, sin asistencia ni supervisión, lo que permite a los autores hacer el trabajo de la próxima semana (o del trimestre siguiente) hoy y no precipitarse en el desarrollo de la página el día antes de que deba publicarse (es verdaderamente el regalo de TIME).

### [Fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments.html)

Los fragmentos de contenido son &quot;fragmentos&quot; personalizables de información que se pueden reutilizar fácilmente en todo el sitio. Si necesita un cambio, solo tiene que cambiar el fragmento original y la actualización se ve en todas partes donde se utiliza, ¡inmediatamente!

### [Fragmentos de experiencias](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

Aunque suenen casi idénticos a los fragmentos de contenido, los fragmentos de experiencias son partes pequeñas y visibles de una página. AEM También se pueden reutilizar ampliamente en todo el sitio y mantenerse en una ubicación central en para facilitar la tarea de realizar cambios potencialmente globales en el sitio en segundos, no en días o semanas.

Piense con anticipación y vea lo que podría reutilizarse. ¿Un pie de página? ¿Una renuncia? ¿Una Cabecera? ¿Ciertos tipos de contenido? Todos estos elementos se pueden compartir en todo un sitio web y, al mismo tiempo, mantener un mantenimiento mínimo. ¿Necesita actualizar una fecha en un descargo de responsabilidad, pero está en 1000 páginas del sitio? Es una operación de 5 segundos si ha utilizado un fragmento de experiencia.

## General

AEM Manténgase al día de los cambios a través del aprendizaje continuo, no se quede atascado en el pasado. Uso [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) y [Adobe Digital Learning Services (ADLS)](https://learning.adobe.com/) para perfeccionar sus habilidades.

## Conclusión

AEM Puede ser un sistema grande, y se necesitan muchos tipos de personas para que &quot;cante&quot;. Desde administradores hasta desarrolladores (tanto desarrolladores de Java front-end como hardcore) y autores: ¡hay algo para todos! AEM Y si no tienes ganas de manejar el día a día de la Administración, siempre hay AMS y as a Cloud Service de la.
