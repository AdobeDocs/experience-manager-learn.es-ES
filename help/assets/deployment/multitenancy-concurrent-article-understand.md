---
title: Comprensión de la multiplicación y el desarrollo simultáneo
seo-title: Comprensión de la multiplicación y el desarrollo simultáneo
description: Obtenga información sobre los beneficios, desafíos y técnicas para administrar una implementación de varios usuarios con Adobe Experience Manager Assets.
uuid: 682093fe-ce55-4ef8-af10-99f7062f8b1b
discoiquuid: 0dfcdf39-7423-459f-8f35-ee5b4b829f2c
feature: connected-assets
topics: authoring, operations, sharing, publishing
audience: all
doc-type: article
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: e03d84f92be11623704602fb448273e461c70b4e
workflow-type: tm+mt
source-wordcount: '2024'
ht-degree: 0%

---


# Explicación de la Multitenencia y el Desarrollo Simultáneo {#understanding-multitenancy-and-concurrent-development}

## Introducción {#introduction}

Cuando varios equipos están implementando su código en los mismos entornos de AEM, hay prácticas que deben seguir para garantizar que los equipos puedan trabajar de la manera más independiente posible, sin pisar los pies de otros equipos. Aunque nunca se pueden eliminar por completo, estas técnicas minimizarán las dependencias entre equipos. Para que un modelo de desarrollo concurrente tenga éxito, es fundamental una buena comunicación entre los equipos de desarrollo.

Además, cuando varios equipos de desarrollo trabajan en el mismo entorno de AEM, es probable que haya un cierto grado de múltiples tenencias en juego. Se ha escrito mucho sobre las consideraciones prácticas de intentar apoyar a varios inquilinos en un entorno AEM, especialmente en torno a los desafíos que se enfrentan al administrar la gobernanza, las operaciones y el desarrollo. En el presente documento se analizan algunos de los problemas técnicos que plantea la aplicación de AEM en un entorno de varios inquilinos, pero muchas de estas recomendaciones se aplicarán a cualquier organización con múltiples equipos de desarrollo.

Es importante destacar que, aunque AEM pueden admitir múltiples sitios e incluso múltiples marcas que se implementan en un solo entorno, no oferta la verdadera capacidad de múltiples tenencias. Algunos recursos de sistemas y configuraciones de entorno siempre se compartirán en todos los sitios que se implementen en un entorno. En el presente documento se ofrece orientación para reducir al mínimo los efectos de esos recursos compartidos y se formulan sugerencias de ofertas para racionalizar la comunicación y la colaboración en esas esferas.

## Beneficios y desafíos {#benefits-and-challenges}

La implementación de un entorno de varios inquilinos plantea muchos desafíos.

Entre estas características se incluyen:

* Complejidad técnica adicional
* Aumento del overhead de desarrollo
* Dependencias entre organizaciones en recursos compartidos
* Mayor complejidad operacional

A pesar de los desafíos a los que se enfrenta, la ejecución de una aplicación de varios inquilinos tiene beneficios, como por ejemplo:

* Menores costos de hardware
* Menor tiempo de salida al mercado para sitios futuros
* Menores costos de implementación para futuros inquilinos
* Arquitectura estándar y prácticas de desarrollo en todo el negocio
* Un código base común

Si el negocio requiere una verdadera multirendencia, con cero conocimiento de otros inquilinos y sin código compartido, contenido o autores comunes, entonces las instancias de autor independientes son la única opción viable. El aumento general del esfuerzo de desarrollo debe compararse con el ahorro en los costos de infraestructura y licencias para determinar si este enfoque es el más adecuado.

## Técnicas de desarrollo {#development-techniques}

### Administración de dependencias {#managing-dependencies}

Al administrar las dependencias del proyecto de Maven, es importante que todos los equipos utilicen la misma versión de un paquete OSGi determinado en el servidor. Para ilustrar lo que puede salir mal cuando los proyectos de Maven están mal administrados, presentamos un ejemplo:

El proyecto A depende de la versión 1.0 de la biblioteca, foo; foo versión 1.0 está incorporado en su implementación en el servidor. El proyecto B depende de la versión 1.1 de la biblioteca, foo; foo versión 1.1 está integrada en su implementación.

Además, supongamos que una API ha cambiado en esta biblioteca entre las versiones 1.0 y 1.1. En este momento, uno de estos dos proyectos ya no funcionará correctamente.

Para abordar esta preocupación, recomendamos que todos los proyectos Maven sean hijos de un proyecto de reactor principal. Este proyecto de reactor tiene dos propósitos: permite la construcción e implementación de todos los proyectos juntos si así lo desea, y contiene las declaraciones de dependencia de todos los proyectos infantiles. El proyecto principal define las dependencias y sus versiones, mientras que los proyectos secundarios solo declaran las dependencias que requieren, heredando la versión del proyecto principal.

En este escenario, si el equipo que trabaja en el Proyecto B requiere funcionalidad en la versión 1.1 de foo, se hará evidente rápidamente en el entorno de desarrollo que este cambio provocará una ruptura en el Proyecto A. En este punto, los equipos pueden discutir este cambio y hacer que el Proyecto A sea compatible con la nueva versión o buscar una solución alternativa para el Proyecto B.

Tenga en cuenta que esto no elimina la necesidad de que estos equipos compartan esta dependencia, simplemente resalta los problemas de manera rápida y temprana para que los equipos puedan discutir cualquier riesgo y acordar una solución.

### Prevención de la duplicación de códigos {#preventing-code-duplication-nbsp-br}

Cuando se trabaja en varios proyectos, es importante asegurarse de que el código no se duplique. La duplicación de código aumenta la probabilidad de que ocurran incidencias de defectos, el costo de los cambios en el sistema y la rigidez general en la base de código. Para evitar la duplicación, refactor la lógica común en bibliotecas reutilizables que se pueden usar en varios proyectos.

Para apoyar esta necesidad, recomendamos el desarrollo y mantenimiento de un proyecto básico del que todos los equipos puedan depender y contribuir. Al hacerlo, es importante garantizar que este proyecto básico no dependa, a su vez, de ninguno de los proyectos de cada equipo; esto permite una capacidad de implementación independiente y, al mismo tiempo, promueve la reutilización del código.

Algunos ejemplos de código que suelen aparecer en un módulo principal son:

* Configuraciones de todo el sistema como:
   * Configuraciones OSGi
   * Filtros de servlet
   * Asignaciones de ResourceResolver
   * Tuberías de transformador Sling
   * Controladores de error (o utilice el controlador de página de error ACS AEM Commons1)
   * Servlets de autorización para almacenamiento en caché que distinguen permisos
* Clases de utilidades
* Lógica básica del negocio
* Lógica de integración de terceros
* Creación de superposiciones de IU
* Otras personalizaciones necesarias para la creación, como las utilidades personalizadas
* Iniciadores de flujo de trabajo
* Elementos de diseño comunes que se utilizan en todos los sitios

*Arquitectura modular del proyecto*

Esto no elimina la necesidad de que varios equipos dependan del mismo conjunto de códigos y lo actualicen potencialmente. Al crear un proyecto principal, hemos reducido el tamaño del código que se comparte entre los equipos, disminuyendo pero sin eliminar la necesidad de recursos compartidos.

Para garantizar que los cambios realizados en este paquete principal no interrumpan la funcionalidad del sistema, recomendamos que un desarrollador o equipo de desarrolladores sénior mantenga la supervisión. Una opción es tener un único equipo que gestione todos los cambios de este paquete; otra es que los equipos envíen solicitudes de extracción que se revisen y combinen con estos recursos. Es importante que los equipos diseñen y acuerden un modelo de gobernanza y que los desarrolladores lo sigan.

## Administración del ámbito de implementación y nbsp {#managing-deployment-scope}

A medida que distintos equipos implementan su código en el mismo repositorio, es importante que no sobrescriban los cambios de los demás. AEM tiene un mecanismo para controlar esto al implementar los paquetes de contenido, el filtro. archivo XML. Es importante que no haya superposición entre filtros.  archivos XML, de lo contrario, la implementación de un equipo podría potencialmente borrar la implementación anterior de otro equipo. Para ilustrar este punto, consulte los siguientes ejemplos de archivos de filtro bien diseñados o problemáticos:

/apps/mi-compañía vs. /apps/my-compañía/my-site

/etc/clientlibs/my-compañía vs. /etc/clientlibs/my-compañía/my-site

/etc/designs/my-compañía vs. /etc/designs/my-compañía/my-site

Si cada equipo configura explícitamente su archivo de filtro en los sitios en los que está trabajando, cada equipo puede implementar sus componentes, bibliotecas de cliente y diseños de sitio de forma independiente sin eliminar los cambios de cada uno de ellos.

Dado que es una ruta de sistema global y no es específica de un sitio, el siguiente servlet debe incluirse en el proyecto principal, ya que los cambios realizados aquí podrían afectar a cualquier equipo:

/apps/sling/servlet/errorhandler

### Superposiciones {#overlays}

Las superposiciones se utilizan con frecuencia para ampliar o reemplazar la funcionalidad de AEM lista, pero el uso de una superposición afecta a toda la aplicación de AEM (es decir, todos los inquilinos podrán disfrutar de cualquier cambio de funcionalidad). Esto se complicaría aún más si los inquilinos tuvieran requisitos diferentes para la superposición. Idealmente, los grupos de negocios deberían trabajar juntos para acordar la funcionalidad y apariencia de las consolas administrativas de AEM.

Si no se puede llegar a un consenso entre las distintas unidades de negocio, una posible solución sería simplemente no utilizar las superposiciones. En su lugar, cree una copia personalizada de la funcionalidad y ejemplifíquela a través de una ruta diferente para cada inquilino. Esto permite que cada inquilino tenga una experiencia de usuario completamente diferente, pero este enfoque también aumenta el costo de la implementación y los posteriores esfuerzos de actualización.

### Lanzadores de flujo de trabajo {#workflow-launchers}

AEM utiliza iniciadores de flujo de trabajo para ejecutar automáticamente el flujo de trabajo de déclencheur cuando se realizan cambios especificados en el repositorio. AEM proporciona varios lanzadores listos para usar, por ejemplo, para ejecutar procesos de generación de representación y extracción de metadatos en recursos nuevos y actualizados. Si bien es posible dejar estos lanzadores tal cual, en un entorno de varios inquilinos, si los inquilinos tienen diferentes requisitos de modelo de lanzador o flujo de trabajo, es probable que sea necesario crear y mantener lanzadores individuales para cada inquilino. Estos lanzadores deberán configurarse para que se ejecuten en las actualizaciones de sus inquilinos, sin modificar el contenido de otros inquilinos. Esto se logra fácilmente mediante la aplicación de lanzadores a rutas de repositorio específicas para cada usuario.

### URL de vanidad {#vanity-urls}

AEM proporciona funcionalidad de URL de vanidad que se puede establecer por página. La preocupación con este enfoque en un escenario de varios inquilinos es que AEM no garantiza la exclusividad entre las direcciones URL personales configuradas de esta manera. Si dos usuarios diferentes configuran la misma ruta de vanidad para páginas diferentes, se puede encontrar un comportamiento inesperado. Por este motivo, recomendamos utilizar las reglas mod_rewrite en las instancias del despachante de Apache, que permiten un punto central de configuración en conjunto con las reglas de resolución de recursos de sólo salida.

### Grupos de componentes {#component-groups}

Al desarrollar componentes y plantillas para varios grupos de creación, es importante utilizar de forma eficaz las propiedades componentGroup y allowPaths. Al aprovechar esto de manera eficaz con los diseños del sitio, podemos asegurarnos de que los autores de la Marca A solo vean los componentes y las plantillas que se han creado para su sitio, mientras que los autores de la Marca B solo verán los suyos.

### Pruebas {#testing}

Aunque una buena arquitectura y canales de comunicación abiertos pueden ayudar a evitar la introducción de defectos en áreas inesperadas del sitio, estos enfoques no son infalibles. Por este motivo, es importante probar completamente lo que se está implementando en la plataforma antes de lanzar cualquier cosa en producción. Esto requiere la coordinación entre los equipos en sus ciclos de lanzamiento y refuerza la necesidad de un conjunto de pruebas automatizadas que cubran la mayor funcionalidad posible. Además, como un sistema será compartido por varios equipos, el rendimiento, la seguridad y las pruebas de carga son más importantes que nunca.

## Consideraciones operacionales {#operational-considerations}

### Recursos compartidos {#shared-resources}

AEM se ejecuta dentro de una sola JVM; cualquier aplicación AEM implementada comparte recursos entre sí de forma inherente, además de los recursos ya consumidos en la ejecución normal de AEM. Dentro del propio espacio JVM, no habrá separación lógica de subprocesos y los recursos finitos disponibles para AEM, como memoria, CPU e i/o de disco, también se compartirán. Cualquier inquilino que consuma recursos afectará inevitablemente a otros inquilinos del sistema.

### Actuación {#performance}

Si no se siguen AEM prácticas recomendadas, es posible desarrollar aplicaciones que consuman recursos por encima de lo que se considera normal. Algunos ejemplos de esto son la activación de muchas operaciones de flujo de trabajo pesadas (como DAM Update Asset), el uso de operaciones de inserción MSM en varios nodos o el uso de consultas de JCR costosas para procesar contenido en tiempo real. Inevitablemente, esto influirá en el rendimiento de otras aplicaciones de inquilinos.

### Registro {#logging}

AEM proporciona interfaces listas para usar para una configuración de registrador robusta que puede utilizarse en nuestro beneficio en escenarios de desarrollo compartidos. Al especificar distintos registradores para cada marca, por nombre del paquete, podemos lograr cierto grado de separación de registros. Aunque las operaciones de todo el sistema, como la replicación y la autenticación, se registrarán en una ubicación central, el código personalizado no compartido se puede registrar por separado, lo que facilita los esfuerzos de supervisión y depuración para cada equipo técnico de la marca.

### Copia de seguridad y restauración {#backup-and-restore}

Debido a la naturaleza del repositorio JCR, los backups tradicionales funcionan en todo el repositorio, en lugar de en una ruta de contenido individual. Por lo tanto, no es posible separar fácilmente las copias de seguridad por usuario. Por el contrario, la restauración desde una copia de seguridad revertirá el contenido y los nodos del repositorio para todos los inquilinos del sistema. Aunque es posible realizar backups de contenido de objetivo, mediante herramientas como VLT, o seleccionar contenido para restaurar mediante la creación de un paquete en un entorno independiente, estas\
los enfoques no abarcan fácilmente la configuración o la lógica de la aplicación y pueden resultar complicados de administrar.

## Consideraciones de seguridad {#security-considerations}

### ACL {#acls}

Por supuesto, es posible utilizar Listas de Control de acceso (ACL) para controlar quién tiene acceso a la vista, la creación y la eliminación de contenido basado en rutas de contenido, lo que requiere la creación y administración de grupos de usuarios. La dificultad para mantener las ACL y los grupos depende del énfasis en garantizar que cada inquilino tenga cero conocimiento de otros y si las aplicaciones implementadas dependen de los recursos compartidos. Para garantizar una administración eficiente de ACL, usuarios y grupos, recomendamos disponer de un grupo centralizado con la supervisión necesaria para garantizar que estos controles de acceso y principios se superpongan (o no se superpongan) de manera que se fomente la eficacia y la seguridad.
