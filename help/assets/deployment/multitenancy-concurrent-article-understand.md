---
title: Explicación del multiinquilino y el desarrollo simultáneo
description: Obtenga información sobre las ventajas, los desafíos y las técnicas para administrar una implementación de varios inquilinos con Adobe Experience Manager Assets.
feature: Connected Assets
version: 6.5
topic: Development
role: Developer
level: Intermediate
doc-type: Article
exl-id: c9ee29d4-a8a5-4e61-bc99-498674887da5
duration: 437
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2022'
ht-degree: 0%

---

# Explicación del multiinquilino y el desarrollo simultáneo {#understanding-multitenancy-and-concurrent-development}

## Introducción {#introduction}

AEM Cuando varios equipos implementan su código en los mismos entornos de trabajo, hay prácticas que deben seguir para garantizar que los equipos puedan trabajar de forma lo más independiente posible, sin pisar los dedos de los pies de otros equipos. Aunque nunca se pueden eliminar por completo, estas técnicas minimizarán las dependencias entre equipos. Para que un modelo de desarrollo simultáneo tenga éxito, es fundamental que haya una buena comunicación entre los equipos de desarrollo.

AEM Además, cuando varios equipos de desarrollo trabajan en el mismo entorno de, es probable que haya cierto grado de multiinquilinos en juego. AEM Se ha escrito mucho sobre las consideraciones prácticas de intentar apoyar a múltiples inquilinos en un entorno de la vida cotidiana, especialmente en torno a los desafíos que enfrentan al momento de administrar la gobernanza, las operaciones y el desarrollo. AEM En este documento se exploran algunos de los desafíos técnicos relacionados con la implementación de la en un entorno de varios inquilinos, pero muchas de estas recomendaciones se aplicarán a cualquier organización con varios equipos de desarrollo.

AEM Es importante tener en cuenta por adelantado que, si bien puede admitir varios sitios e incluso varias marcas que se implementen en un solo entorno, no ofrece una verdadera capacidad de inquilino múltiple. Algunas configuraciones de entorno y recursos de sistemas siempre se compartirán en todos los sitios implementados en un entorno. Este documento proporciona orientación para minimizar los efectos de estos recursos compartidos y ofrece sugerencias para racionalizar la comunicación y la colaboración en estas áreas.

## Ventajas y desafíos {#benefits-and-challenges}

La implementación de un entorno de varios inquilinos presenta muchos desafíos.

Entre estas características se incluyen:

* Complejidad técnica adicional
* Mayor sobrecarga de desarrollo
* Dependencias entre organizaciones en un recurso compartido
* Mayor complejidad operativa

A pesar de los desafíos a los que se enfrenta, ejecutar una aplicación de varios inquilinos sí tiene ventajas, como las siguientes:

* Costes de hardware reducidos
* Menor tiempo de comercialización para sitios futuros
* Menores costes de implementación para futuros inquilinos
* Arquitectura estándar y prácticas de desarrollo en toda la empresa
* Una base de código común

Si el negocio requiere un verdadero inquilino múltiple, con cero conocimiento de otros inquilinos y sin código compartido, contenido o autores comunes, entonces las instancias de autor independientes son la única opción viable. El aumento general del esfuerzo de desarrollo debe compararse con el ahorro en costes de infraestructura y licencias para determinar si este enfoque es el más adecuado.

## Técnicas de desarrollo {#development-techniques}

### Administración de dependencias {#managing-dependencies}

Al administrar las dependencias del proyecto Maven, es importante que todos los equipos utilicen la misma versión de un paquete OSGi determinado en el servidor. Para ilustrar qué puede salir mal cuando los proyectos de Maven se administran mal, presentamos un ejemplo:

El proyecto A depende de la versión 1.0 de la biblioteca, foo; la versión 1.0 de foo está incrustada en su implementación en el servidor. El proyecto B depende de la versión 1.1 de la biblioteca, foo; la versión 1.1 de foo está incrustada en su implementación.

Además, supongamos que una API ha cambiado en esta biblioteca entre las versiones 1.0 y 1.1. En este punto, uno de estos dos proyectos dejará de funcionar correctamente.

Para resolver esta preocupación, recomendamos hacer que todos los proyectos de Maven sean hijos de un proyecto de reactor principal. Este proyecto de reactor tiene dos propósitos: permite la generación e implementación de todos los proyectos juntos si se desea y contiene las declaraciones de dependencia para todos los proyectos secundarios. El proyecto principal define las dependencias y sus versiones, mientras que los proyectos secundarios solo declaran las dependencias que requieren, heredando la versión del proyecto principal.

En este escenario, si el equipo que trabaja en el proyecto B requiere funcionalidad en la versión 1.1 de foo, rápidamente se hará evidente en el entorno de desarrollo que este cambio romperá el proyecto A. En este punto, los equipos pueden analizar este cambio y hacer que el proyecto A sea compatible con la nueva versión o buscar una solución alternativa para el proyecto B.

Tenga en cuenta que esto no elimina la necesidad de que estos equipos compartan esta dependencia, solo resalta los problemas de forma rápida y temprana para que los equipos puedan analizar cualquier riesgo y acordar una solución.

### Prevención de la duplicación de código {#preventing-code-duplication-nbsp-br}

Cuando se trabaja en varios proyectos, es importante asegurarse de que el código no esté duplicado. La duplicación de código aumenta la probabilidad de que ocurran defectos, el coste de los cambios en el sistema y la rigidez general en la base de código. Para evitar duplicaciones, refactorice la lógica común en bibliotecas reutilizables que se pueden utilizar en varios proyectos.

Para satisfacer esta necesidad, recomendamos el desarrollo y mantenimiento de un proyecto principal en el que todos los equipos puedan depender y contribuir. Al hacerlo, es importante asegurarse de que este proyecto principal no dependa, a su vez, de ninguno de los proyectos de los equipos individuales; esto permite una implementación independiente, a la vez que promueve la reutilización del código.

Algunos ejemplos de código que suelen incluirse en un módulo principal son:

* Configuraciones en todo el sistema como:
   * Configuraciones de OSGi
   * Filtros Servlet
   * Asignaciones de ResourceResolver
   * Canalizaciones del transformador Sling
   * AEM Controladores de error (o use el Controlador de página de error 1 de ACS Commons)
   * Servlets de autorización para el almacenamiento en caché con permisos confidenciales
* Clases de utilidad
* Lógica empresarial principal
* Lógica de integración de terceros
* Creación de superposiciones de IU
* Otras personalizaciones necesarias para la creación, como los widgets personalizados
* Iniciadores de flujo de trabajo
* Elementos de diseño comunes que se utilizan en los sitios

*Arquitectura modular del proyecto*

Esto no elimina la necesidad de que varios equipos dependan y puedan actualizar el mismo conjunto de código. Al crear un proyecto principal, hemos reducido el tamaño del código base que se comparte entre equipos, lo que disminuye, pero no elimina la necesidad de compartir recursos.

Para garantizar que los cambios realizados en este paquete principal no interrumpan la funcionalidad del sistema, recomendamos que un desarrollador sénior o un equipo de desarrolladores mantengan la supervisión. Una opción es que haya un solo equipo que administre todos los cambios realizados en este paquete; otra es que los equipos envíen solicitudes de extracción que se revisen y fusionen con estos recursos. Es importante que los equipos diseñen y acepten un modelo de gobernanza y que los desarrolladores lo sigan.

## Administrar el ámbito de implementación {#managing-deployment-scope}

A medida que diferentes equipos implementan su código en el mismo repositorio, es importante que no sobrescriban los cambios de los demás. AEM tiene un mecanismo para controlar esto al implementar paquetes de contenido, el filtro. archivo xml. Es importante que no haya superposición entre filtros.  archivos xml; de lo contrario, la implementación de un equipo podría borrar la implementación anterior de otro equipo. Para ilustrar este punto, consulte los siguientes ejemplos de archivos de filtro bien diseñados o problemáticos:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company frente a /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Si cada equipo configura explícitamente su archivo de filtro hasta el sitio en el que está trabajando, cada equipo puede implementar sus componentes, bibliotecas de cliente y diseños de sitio de forma independiente sin borrar los cambios de los demás.

Como es una ruta del sistema global y no es específica de un sitio, el siguiente servlet debe incluirse en el proyecto principal, ya que los cambios realizados aquí podrían afectar a cualquier equipo:

/apps/sling/servlet/errorhandler

### Superposiciones {#overlays}

AEM AEM Las superposiciones se utilizan frecuentemente para ampliar o reemplazar la funcionalidad de la aplicación de forma predeterminada, pero el uso de una superposición afecta a toda la aplicación de la aplicación (es decir, cualquier cambio de funcionalidad superpuesta está disponible para todos los inquilinos). Esto sería aún más complicado si los inquilinos tuvieran diferentes requisitos para la superposición. AEM Lo ideal sería que los grupos empresariales trabajaran juntos para acordar la funcionalidad y el aspecto de las consolas administrativas de las que se dispone en la administración de los usuarios de la red de distribución de la información y las comunicaciones.

Si no se puede llegar a un consenso entre las distintas unidades de negocio, una posible solución sería simplemente no utilizar superposiciones. En su lugar, cree una copia personalizada de la funcionalidad y expóngala a través de una ruta diferente para cada inquilino. Esto permite que cada inquilino tenga una experiencia de usuario completamente diferente, pero este método también aumenta el coste de la implementación y los esfuerzos de actualización posteriores.

### Lanzadores de flujo de trabajo {#workflow-launchers}

AEM utiliza iniciadores de flujo de trabajo para almacenar automáticamente en déclencheur la ejecución del flujo de trabajo cuando se realizan cambios específicos en el repositorio. AEM proporciona varios iniciadores predeterminados, por ejemplo, para ejecutar procesos de generación de representaciones y extracción de metadatos en recursos nuevos y actualizados. Aunque es posible dejar estos lanzadores tal cual, en un entorno de varios inquilinos, si los inquilinos tienen diferentes requisitos de modelo de lanzador o flujo de trabajo, es probable que se deban crear y mantener lanzadores individuales para cada inquilino. Estos iniciadores deberán configurarse para que se ejecuten en las actualizaciones de su inquilino, sin modificar el contenido de otros inquilinos. Esto se consigue fácilmente aplicando iniciadores a rutas de repositorio especificadas específicas del inquilino.

### URL de vanidad {#vanity-urls}

AEM proporciona funcionalidad de URL de vanidad que se puede configurar por página. AEM La preocupación con este enfoque en un escenario de varios inquilinos es que no se garantiza la exclusividad entre las URL de vanidad configuradas de esta manera. Si dos usuarios diferentes configuran la misma ruta de vanidad para páginas diferentes, se puede encontrar un comportamiento inesperado. Por este motivo, recomendamos utilizar reglas mod_rewrite en las instancias de Apache Dispatcher, que permiten un punto central de configuración junto con las reglas de resolución de recursos de solo salida.

### Grupos de componentes {#component-groups}

Al desarrollar componentes y plantillas para varios grupos de creación, es importante utilizar de forma eficaz las propiedades componentGroup y allowedPaths. Al aprovechar estos componentes de forma eficaz con los diseños de sitio, podemos garantizar que los autores de la marca A solo vean los componentes y las plantillas que se han creado para su sitio, mientras que los autores de la marca B solo verán los suyos.

### Pruebas {#testing}

Si bien una buena arquitectura y canales de comunicación abiertos pueden ayudar a evitar la introducción de defectos en áreas inesperadas del sitio, estos enfoques no son infalibles. Por este motivo, es importante probar completamente lo que se está implementando en la plataforma antes de lanzar cualquier elemento en producción. Esto requiere una coordinación entre los equipos en sus ciclos de lanzamiento y refuerza la necesidad de un conjunto de pruebas automatizadas que cubran la mayor funcionalidad posible. Además, dado que varios equipos comparten un sistema, las pruebas de rendimiento, seguridad y carga cobran mayor importancia que nunca.

## Consideraciones operativas {#operational-considerations}

### Recursos compartidos {#shared-resources}

AEM AEM AEM se ejecuta dentro de una sola JVM; todas las aplicaciones de la aplicación de la implementación comparten recursos entre sí de forma inherente, además de los recursos que ya se consumen durante la ejecución normal de las aplicaciones de la implementación de la aplicación de la plataforma de administración de recursos (JVMs). AEM Dentro del propio espacio JVM, no hay una separación lógica de subprocesos y los recursos finitos disponibles para los usuarios, como la memoria, la CPU y la E/S del disco, también se comparten. Cualquier inquilino que consuma recursos afectará inevitablemente a otros inquilinos del sistema.

### Rendimiento {#performance}

AEM Si no se siguen las prácticas recomendadas de la aplicación, es posible desarrollar aplicaciones que consuman recursos superiores a los que se consideran normales. Algunos ejemplos de esto son la activación de muchas operaciones de flujo de trabajo pesadas (como el recurso de actualización DAM), el uso de operaciones push-on-modify de MSM en muchos nodos o el uso de consultas JCR costosas para procesar contenido en tiempo real. Esto inevitablemente tendrá un impacto en el rendimiento de otras aplicaciones de inquilino.

### Registro {#logging}

AEM proporciona interfaces listas para usar para una configuración de registrador sólida que se puede utilizar en nuestra ventaja en escenarios de desarrollo compartidos. Al especificar registradores independientes para cada marca, por nombre de paquete, podemos lograr cierto grado de separación de registros. Aunque las operaciones de todo el sistema, como la replicación y la autenticación, se registrarán en una ubicación central, el código personalizado no compartido se puede registrar por separado, lo que facilita los esfuerzos de monitorización y depuración del equipo técnico de cada marca.

### Copia de seguridad y restauración {#backup-and-restore}

Debido a la naturaleza del repositorio JCR, las copias de seguridad tradicionales funcionan en todo el repositorio, en lugar de en una ruta de contenido individual. Por lo tanto, no es posible separar fácilmente las copias de seguridad por inquilino. Por el contrario, la restauración desde una copia de seguridad revertirá el contenido y los nodos del repositorio para todos los inquilinos del sistema. Aunque es posible realizar copias de seguridad de contenido de destino con herramientas como VLT o elegir contenido para restaurarlo mediante la creación de un paquete en un entorno independiente, estas son\
los enfoques no engloban fácilmente los ajustes de configuración o la lógica de la aplicación y pueden ser engorrosos de administrar.

## Consideraciones de seguridad {#security-considerations}

### ACL {#acls}

Por supuesto, es posible utilizar Listas de control de acceso (ACL) para controlar quién tiene acceso para ver, crear y eliminar contenido basado en rutas de contenido, lo que requiere la creación y administración de grupos de usuarios. La dificultad para mantener las ACL y los grupos depende del énfasis en garantizar que cada inquilino tenga cero conocimiento de los demás y si las aplicaciones implementadas dependen de recursos compartidos. Para garantizar una administración eficiente de ACL, usuarios y grupos, recomendamos tener un grupo centralizado con la supervisión necesaria para garantizar que estos controles de acceso y principales se superpongan (o no se superpongan) de una manera que promueva la eficiencia y la seguridad.
