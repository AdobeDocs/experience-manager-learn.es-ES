---
title: Consejos y trucos para el mantenimiento del sitio
description: Consejos y trucos para el mantenimiento del sitio
hide: true
hidefromtoc: true
source-git-commit: 3eb429039589ae26a81bc6d24f020a77517133e8
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 4%

---


# Consejos y trucos para el mantenimiento del sitio

Existen tres opciones para instalar y mantener una instancia de AEM

* AEMaaCS (servicio en la nube): el sistema siempre está activado, actualizado y se adapta dinámicamente según lo necesite
* Servicios administrados de Adobe, donde los ingenieros de servicio al cliente de Adobe realizan todas las labores de mantenimiento diarias, semanales y mensuales, y se aseguran de que todos los paquetes de servicios estén instalados y de que el sistema esté siempre seguro y funcionando sin problemas
* ejecutarlo in situ, donde debe encargarse de todo el sistema, incluidos backups, actualizaciones y seguridad.

Si decide implementar su propio sistema en las instalaciones, hay que tener en cuenta algunas cosas para garantizar que dispone de un sistema seguro y eficaz. Además de los artículos de &quot;cuidado y alimentación&quot;, este documento también señalará varios artículos AEM los desarrolladores deben tener en cuenta para ayudar a mantener el sistema funcionando bien.

## Administradores

Copias de seguridad: asegúrese de tener copias de seguridad completas o parciales en un programa frecuente:

* cada día
* cada semana
* mensual

Muchos clientes realizan copias de seguridad instantáneas, que sólo tardan unos minutos en asumir que el sistema operativo subyacente admite dichas copias de seguridad. Asegúrese de que estas copias de seguridad estén almacenadas correctamente (fuera del sistema AEM). Asegúrese de que los backups son funcionales y pueden utilizarse para recrear un sistema de trabajo periódicamente. No hay nada peor que tener el sistema bloqueado y sus backups son corruptos por alguna razón.

Hay varios elementos que debe monitorizar para garantizar el funcionamiento sin problemas:

### Mantenimiento rutinario

#### [mantenimiento del índice](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=es)

Los índices permiten que las consultas se ejecuten lo más rápido posible, lo que libera recursos para otras operaciones. ¡Asegúrese de que sus índices están en la forma punta! AEM cancela las consultas que atraviesan en lugar de utilizar un índice para evitar que una consulta incorrecta afecte al rendimiento general de la AEM.

#### [Compactación de tar/limpieza de revisión](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Cada actualización del repositorio crea una nueva revisión de contenido. Como resultado, con cada actualización, el tamaño del repositorio crece. Para evitar el crecimiento incontrolado del repositorio, es necesario limpiar las viejas revisiones para liberar recursos de disco.

#### [Limpieza de binarios de Lucene](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-dashboard.html?lang=en#automated-maintenance-tasks)

Purgue los binarios de lucene y reduzca el requisito de tamaño del almacén de datos en ejecución.

#### [Basura del almacén de datos](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/data-store-garbage-collection.html?lang=en)

Cuando se elimina un recurso de AEM, la referencia al registro del almacén de datos subyacente se puede eliminar de la jerarquía de nodos, pero el registro del almacén de datos en sí permanece. Este registro de almacén de datos sin referencia se convierte en &quot;basura&quot; que no necesita retenerse. En los casos en los que existen varios activos sin referencia, es beneficioso deshacerse de ellos, conservar el espacio, optimizar el backup y el performance de mantenimiento del filesystem.

#### [Depuración de flujo de trabajo](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/workflows-administering.html?lang=en)

Al minimizar el número de instancias de flujo de trabajo, aumenta el rendimiento del motor de flujo de trabajo, por lo que puede depurar con regularidad las instancias de flujo de trabajo completadas o en ejecución desde el repositorio.

#### [Mantenimiento del registro de auditoría](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-audit-log.html?lang=en)

Los eventos de AEM que cumplen los requisitos para el registro de auditoría generan una gran cantidad de datos archivados. Estos datos pueden crecer rápidamente con el tiempo debido a las réplicas, cargas de recursos y otras actividades del sistema.

#### [Seguridad](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=en)

Asegúrese de que las prácticas recomendadas de la lista de comprobación de seguridad se siguen de cerca para garantizar la instancia más segura de AEM.

#### Diskspace

Supervise el espacio en disco para asegurarse de que tiene suficiente para el Repositorio JCR, además de la mitad de espacio adicional, la compactación tar utiliza espacio adicional cuando se ejecuta. ¡Quedarse sin espacio en disco es la razón número uno de la corrupción de JCR!

## Desarrollador

Intente no usar componentes personalizados; use [componentes principales](https://www.aemcomponents.dev/). Su objetivo debe ser utilizar los componentes principales del 80 al 90 % del tiempo y los componentes personalizados solo con moderación. Esto suele requerir una nueva forma de ver los componentes en una página. Debe tener en cuenta que un desarrollador front-end que utiliza CSS puede volver a administrar los componentes fácilmente. Además, tenga en cuenta que esos componentes básicos pueden integrarse entre sí para lograr resultados bastante complejos. ¡Consigue creatividad!

### [Sistemas de estilos](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

Los sistemas de estilos permiten que los componentes principales, e incluso los componentes personalizados, cambien su aspecto a discreción de los autores para crear componentes de aspecto completamente nuevo. Estos cambios estilísticos generalmente implican únicamente a un diseñador de front-end y a un autor experto (a menudo denominado &quot;superautor&quot;)

### [Lanzamientos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

Los lanzamientos permiten que el trabajo se complete para una nueva promoción, venta o implementación de sitio web sin afectar a las páginas implementadas actualmente. Además, se pueden programar para que se activen automáticamente, sin asistencia ni supervisión, lo que permite a los autores hacer el trabajo de la semana que viene (o del trimestre siguiente) hoy y no apresurarse en el desarrollo de la página el día antes de que se ponga en marcha - ¡es realmente el regalo de TIME!)

### [Fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-64/assets/fragments/content-fragments.html?lang=en)

Los fragmentos de contenido son &quot;fragmentos&quot; personalizables de información que se pueden reutilizar fácilmente en todo el sitio. Si necesitas un cambio, solo cambias el fragmento original y la actualización se ve en todas partes donde se usa - inmediatamente!

### [Fragmentos de experiencias](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

Aunque los fragmentos de experiencias suenan casi idénticos a los fragmentos de contenido, son fragmentos pequeños y visibles de una página. Estas también se pueden reutilizar ampliamente en el sitio y mantener en una ubicación central dentro de AEM para facilitar la tarea de realizar posibles cambios globales en el sitio en segundos, no en días o semanas.

Piensen en el futuro y vean lo que podría reutilizarse. ¿Un pie de página? ¿Una renuncia de responsabilidad? ¿Un encabezado? ¿Ciertos tipos de contenido? Todo esto se puede compartir en un sitio completo manteniendo el mantenimiento como mínimo. ¿Necesita actualizar una fecha en una renuncia de responsabilidad, pero está en 1000 páginas en el sitio? Se trata de una operación de 5 segundos si se ha utilizado un fragmento de experiencia.

## General

Manténgase al tanto de los cambios AEM a través del aprendizaje continuo - no se atasque en el pasado. Uso [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) y [Servicios de aprendizaje digital de Adobe (ADLS)](https://learning.adobe.com/) para perfeccionar sus habilidades.

## Conclusión

AEM puede ser un sistema grande, y se necesita que muchos tipos de personas lo hagan &quot;cantar&quot;. Desde administradores a desarrolladores (tanto desarrolladores de Java front-end como de hardcore) hasta autores, hay algo para todos. Y si no tienes ganas de manejar el día a día Administración, siempre hay AMS y AEM as a Cloud Service.
