---
title: Comprensión de la multiplicación y el desarrollo concurrente
description: Obtenga información sobre las ventajas, los desafíos y las técnicas para administrar una implementación de varios usuarios con Adobe Experience Manager Assets.
feature: Recursos conectados
version: 6.5
topic: Desarrollo
role: Desarrollador
level: Intermedio
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2024'
ht-degree: 0%

---


# Comprensión de la Multitenencia y el Desarrollo Simultáneo {#understanding-multitenancy-and-concurrent-development}

## Introducción {#introduction}

Cuando varios equipos implementan su código en los mismos entornos de AEM, hay prácticas que deben seguir para garantizar que los equipos puedan trabajar de la manera más independiente posible, sin pisar los pies de otros equipos. Aunque nunca se pueden eliminar por completo, estas técnicas minimizarán las dependencias entre equipos. Para que un modelo de desarrollo concurrente tenga éxito, es fundamental que exista una buena comunicación entre los equipos de desarrollo.

Además, cuando varios equipos de desarrollo trabajan en el mismo entorno de AEM, es probable que haya un cierto grado de inquilino múltiple en juego. Se ha escrito mucho sobre las consideraciones prácticas de intentar apoyar a varios inquilinos en un entorno AEM, especialmente en torno a los desafíos que enfrentan al administrar la gobernanza, las operaciones y el desarrollo. En este documento se analizan algunos de los desafíos técnicos relacionados con la implementación de AEM en un entorno de varios usuarios, pero muchas de estas recomendaciones se aplicarán a cualquier organización con varios equipos de desarrollo.

Es importante señalar en primer lugar que, aunque AEM puede admitir varios sitios e incluso marcas múltiples que se implementan en un solo entorno, no ofrece una verdadera inquilina múltiple. Algunas configuraciones de entorno y recursos de sistemas siempre se compartirán entre todos los sitios implementados en un entorno. El presente documento ofrece orientación para reducir al mínimo los efectos de esos recursos compartidos y ofrece sugerencias para racionalizar la comunicación y la colaboración en esas esferas.

## Ventajas y desafíos {#benefits-and-challenges}

La implementación de un entorno de varios inquilinos presenta muchos desafíos.

Entre estas características se incluyen:

* Complejidad técnica adicional
* Mayor sobrecarga de desarrollo
* Dependencias entre organizaciones en un recurso compartido
* Mayor complejidad operativa

A pesar de los desafíos a los que se enfrenta, la ejecución de una aplicación de varios inquilinos sí tiene beneficios, como:

* Menores costos de hardware
* Reducción del tiempo de salida al mercado para sitios futuros
* Menores costos de implementación para futuros inquilinos
* Arquitectura estándar y prácticas de desarrollo en todo el negocio
* Un código base común

Si el negocio requiere verdadera inquilina múltiple, con cero conocimiento de otros inquilinos y ningún código compartido, contenido o autores comunes, entonces las instancias de autor separadas son la única opción viable. El aumento general del esfuerzo de desarrollo debe compararse con el ahorro en los costos de infraestructura y licencia para determinar si este enfoque es el más adecuado.

## Técnicas de desarrollo {#development-techniques}

### Administración de dependencias {#managing-dependencies}

Al administrar las dependencias del proyecto Maven, es importante que todos los equipos utilicen la misma versión de un paquete OSGi determinado en el servidor. Para ilustrar lo que puede salir mal cuando los proyectos de Maven están mal administrados, presentamos un ejemplo:

El proyecto A depende de la versión 1.0 de la biblioteca, foo; foo versión 1.0 está incrustado en su implementación en el servidor. El proyecto B depende de la versión 1.1 de la biblioteca, foo; foo versión 1.1 está incrustado en su implementación.

Además, supongamos que una API ha cambiado en esta biblioteca entre las versiones 1.0 y 1.1. En este punto, uno de estos dos proyectos ya no funcionará correctamente.

Para abordar esta preocupación, recomendamos que todos los proyectos Maven sean hijos de un proyecto de reactor principal. Este proyecto de reactor tiene dos propósitos: permite la construcción e implementación de todos los proyectos juntos si así se desea, y contiene las declaraciones de dependencia para todos los proyectos secundarios. El proyecto principal define las dependencias y sus versiones, mientras que los proyectos secundarios solo declaran las dependencias que requieren, heredando la versión del proyecto principal.

En este escenario, si el equipo que trabaja en el proyecto B requiere funcionalidad en la versión 1.1 de foo, rápidamente se hará evidente en el entorno de desarrollo que este cambio romperá el proyecto A. En este punto, los equipos pueden discutir este cambio y hacer que el Proyecto A sea compatible con la nueva versión o buscar una solución alternativa para el Proyecto B.

Tenga en cuenta que esto no elimina la necesidad de que estos equipos compartan esta dependencia, solo resalta los problemas de forma rápida y temprana para que los equipos puedan discutir cualquier riesgo y acordar una solución.

### Prevención de la duplicación de código {#preventing-code-duplication-nbsp-br}

Cuando se trabaja en varios proyectos, es importante asegurarse de que el código no esté duplicado. La duplicación de código aumenta la probabilidad de ocurrencias de defectos, el costo de los cambios en el sistema y la rigidez general en la base de código. Para evitar la duplicación, refactorice la lógica común en bibliotecas reutilizables que se pueden usar en varios proyectos.

Para satisfacer esta necesidad, recomendamos el desarrollo y mantenimiento de un proyecto básico del que todos los equipos puedan depender y contribuir. Al hacerlo, es importante garantizar que este proyecto básico no dependa, a su vez, de ninguno de los proyectos de cada equipo; esto permite una capacidad de implementación independiente y, a la vez, promueve la reutilización del código.

Algunos ejemplos de código que normalmente se encuentran en un módulo principal son:

* Configuraciones de todo el sistema como:
   * Configuraciones de OSGi
   * Filtros servlet
   * Asignaciones de ResourceResolver
   * Canalizaciones Sling Transformer
   * Controladores de errores (o use el gestor de páginas de error ACS AEM Commons 1)
   * Servlets de autorización para almacenamiento en caché que distingue entre permisos
* Clases de utilidad
* Lógica principal de la empresa
* Lógica de integración de terceros
* Creación de superposiciones de IU
* Otras personalizaciones necesarias para la creación, como los widgets personalizados
* Iniciadores de flujo de trabajo
* Elementos de diseño comunes que se utilizan en todos los sitios

*Arquitectura de proyecto modular*

Esto no elimina la necesidad de que varios equipos dependan del mismo conjunto de código y lo actualicen potencialmente. Al crear un proyecto principal, hemos reducido el tamaño del código base que se comparte entre los equipos, disminuyendo pero no eliminando la necesidad de recursos compartidos.

Para garantizar que los cambios realizados en este paquete principal no interrumpan la funcionalidad del sistema, se recomienda que un desarrollador o equipo de desarrolladores de categoría superior mantenga la supervisión. Una opción es tener un único equipo que administre todos los cambios a este paquete; otro es que los equipos envíen solicitudes de extracción que se revisen y fusionen con estos recursos. Es importante que los equipos diseñen y acuerden un modelo de gobernanza y que los desarrolladores lo sigan.

## Administración del ámbito de implementación y nbsp {#managing-deployment-scope}

A medida que distintos equipos implementan su código en el mismo repositorio, es importante que no sobrescriban los cambios de los demás. AEM tiene un mecanismo para controlar esto al implementar paquetes de contenido, el filtro. archivo xml. Es importante que no haya superposición entre filtros.  archivos xml, de lo contrario, la implementación de un equipo podría borrar la implementación anterior de otro equipo. Para ilustrar este punto, consulte los siguientes ejemplos de archivos de filtro bien diseñados o problemáticos:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs. /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Si cada equipo configura explícitamente su archivo de filtro según los sitios en los que esté trabajando, cada equipo puede implementar sus componentes, bibliotecas de cliente y diseños de sitio de forma independiente sin necesidad de borrar los cambios de los demás.

Dado que es una ruta de sistema global y no es específica de un sitio, el siguiente servlet debe incluirse en el proyecto principal, ya que los cambios realizados aquí podrían afectar a cualquier equipo:

/apps/sling/servlet/errorhandler

### Superposiciones {#overlays}

Las superposiciones se utilizan frecuentemente para ampliar o reemplazar la funcionalidad de AEM predeterminada, pero el uso de una superposición afecta a toda la aplicación de AEM (es decir, todos los inquilinos tendrán acceso a los cambios de funcionalidad). Esto se complicaría aún más si los inquilinos tuvieran requisitos diferentes para la superposición. Lo ideal es que los grupos de negocios trabajen juntos para acordar la funcionalidad y apariencia de las consolas administrativas de AEM.

Si no se puede llegar a un consenso entre las distintas unidades de negocio, una posible solución sería simplemente no utilizar superposiciones. En su lugar, cree una copia personalizada de la funcionalidad y expárela a través de una ruta diferente para cada inquilino. Esto permite que cada inquilino tenga una experiencia de usuario completamente diferente, pero este enfoque también aumenta el coste de la implementación y los posteriores esfuerzos de actualización.

### Lanzadores de flujo de trabajo {#workflow-launchers}

AEM utiliza iniciadores de flujo de trabajo para activar automáticamente la ejecución del flujo de trabajo cuando se realizan los cambios especificados en el repositorio. AEM proporciona varios lanzadores listos para usar, por ejemplo, para ejecutar procesos de generación de representación y extracción de metadatos en recursos nuevos y actualizados. Si bien es posible dejar estos lanzadores tal cual, en un entorno de varios inquilinos, si los inquilinos tienen diferentes requisitos de modelo de lanzador o flujo de trabajo, es probable que haya que crear y mantener lanzadores individuales para cada inquilino. Estos iniciadores deberán configurarse para que se ejecuten en las actualizaciones de sus inquilinos sin modificar el contenido de otros inquilinos. Lleve a cabo esto fácilmente mediante la aplicación de lanzadores a rutas de repositorio especificadas que son específicas del inquilino.

### URL de vanidad {#vanity-urls}

AEM proporciona funcionalidad de URL de vanidad que se puede establecer por página. La preocupación con este enfoque en un escenario de varios inquilinos es que AEM no garantiza la exclusividad entre las URL personales configuradas de esta manera. Si dos usuarios diferentes configuran la misma ruta de vanidad para páginas diferentes, se puede encontrar un comportamiento inesperado. Por este motivo, recomendamos utilizar reglas mod_rewrite en las instancias de Dispatcher de Apache, que permiten un punto central de configuración de forma conjunta con reglas de Resource Resolver de solo salida.

### Grupos de componentes {#component-groups}

Al desarrollar componentes y plantillas para varios grupos de creación, es importante utilizar de forma eficaz las propiedades componentGroup y allowedPaths . Al aprovechar estos datos de forma eficaz con los diseños de sitio, podemos asegurarnos de que los autores de Marca A solo vean los componentes y las plantillas que se han creado para su sitio, mientras que los autores de Marca B solo verán los suyos.

### Pruebas {#testing}

Aunque una buena arquitectura y canales de comunicación abiertos pueden ayudar a evitar la introducción de defectos en áreas inesperadas del sitio, estos enfoques no son infalibles. Por este motivo, es importante probar completamente lo que se está implementando en la plataforma antes de lanzar cualquier cosa en producción. Esto requiere la coordinación entre los equipos en sus ciclos de lanzamiento y refuerza la necesidad de un conjunto de pruebas automatizadas que cubran la mayor funcionalidad posible. Además, como un sistema será compartido por varios equipos, el rendimiento, la seguridad y las pruebas de carga se vuelven más importantes que nunca.

## Consideraciones operativas {#operational-considerations}

### Recursos compartidos {#shared-resources}

AEM se ejecuta en una sola JVM; todas las aplicaciones de AEM implementadas comparten recursos entre sí, además de los recursos ya consumidos en la ejecución normal de AEM. Dentro del propio espacio de JVM, no habrá una separación lógica de los subprocesos, y los recursos finitos disponibles para AEM, como memoria, CPU e i/o de disco también se compartirán. Cualquier inquilino que consuma recursos afectará inevitablemente a otros inquilinos del sistema.

### Actuación {#performance}

Si no se siguen las prácticas recomendadas de AEM, es posible desarrollar aplicaciones que consuman recursos por encima de lo que se considera normal. Algunos ejemplos de esto son la activación de muchas operaciones de flujo de trabajo pesadas (como DAM Update Asset), el uso de operaciones push de MSM en muchos nodos o el uso de consultas JCR costosas para procesar contenido en tiempo real. Esto inevitablemente tendrá un impacto en el rendimiento de otras aplicaciones de inquilino.

### Registro {#logging}

AEM proporciona interfaces listas para usar para una configuración de registrador robusta que puede utilizarse en nuestro beneficio en escenarios de desarrollo compartidos. Al especificar registros separados para cada marca, por nombre de paquete, podemos lograr cierto grado de separación de registros. Aunque las operaciones de todo el sistema, como la replicación y la autenticación, se registrarán en una ubicación central, el código personalizado no compartido se puede registrar por separado, lo que facilita los esfuerzos de supervisión y depuración para el equipo técnico de cada marca.

### Copia de seguridad y restauración {#backup-and-restore}

Debido a la naturaleza del repositorio JCR, los backups tradicionales funcionan en todo el repositorio, en lugar de en una ruta de contenido individual. Por lo tanto, no es posible separar fácilmente los backups por cada inquilino. Por el contrario, la restauración desde una copia de seguridad restaurará el contenido y los nodos del repositorio para todos los inquilinos del sistema. Aunque es posible realizar copias de seguridad de contenido dirigidas, utilizando herramientas como VLT, o seleccionar contenido para restaurarlo creando un paquete en un entorno separado, estas\
los enfoques no abarcan fácilmente los ajustes de configuración o la lógica de aplicación y pueden resultar engorrosos de administrar.

## Consideraciones de seguridad {#security-considerations}

### ACL {#acls}

Por supuesto, es posible utilizar Listas de control de acceso (ACL) para controlar quién tiene acceso a ver, crear y eliminar contenido en función de las rutas de contenido, lo que requiere la creación y administración de grupos de usuarios. La dificultad para mantener las ACL y los grupos depende del énfasis en garantizar que cada inquilino tenga cero conocimiento de otros y si las aplicaciones implementadas dependen de recursos compartidos. Para garantizar una administración eficiente de ACL, usuarios y grupos, recomendamos tener un grupo centralizado con la supervisión necesaria para garantizar que estos controles y principios de acceso se superpongan (o no se superpongan) de una manera que promueva la eficiencia y la seguridad.
