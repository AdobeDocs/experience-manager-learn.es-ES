---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# AEM implementaciones sin encabezado

AEM implementaciones de cliente sin encabezado adoptan muchas formas; SPA alojadas en AEM, SPA externa o sitio web, aplicación móvil o incluso proceso de servidor a servidor.

Según el cliente y cómo se implemente, AEM implementaciones sin encabezado tienen diferentes consideraciones.

## arquitectura de servicio AEM

Antes de explorar las consideraciones de implementación, es imperativo comprender AEM arquitectura lógica y la separación y las funciones de los niveles de servicio de AEM as a Cloud Service. AEM as a Cloud Service consta de dos servicios lógicos:

+ __Autor de AEM__ es el servicio en el que los equipos crean, colaboran y publican fragmentos de contenido (y otros recursos)
+ __AEM Publish__ es el servicio en el que se replican los fragmentos de contenido publicados (y otros recursos) para un consumo general

AEM clientes sin encabezado que operan con capacidad de producción normalmente interactúan con AEM Publish, que contiene el contenido aprobado y publicado. El cliente que interactúa con AEM Author debe tener especialmente en cuenta, ya que AEM Author es seguro de forma predeterminada, lo que requiere autorización para todas las solicitudes, además de contener contenido incompleto o no aprobado.

## Implementaciones de cliente sin encabezado

+ [Aplicación de una sola página](./spa.md)
+ [Componente web/JS](./web-component.md)
+ [Aplicación móvil](./mobile.md)
+ [Servidor a servidor](./server-to-server.md)

