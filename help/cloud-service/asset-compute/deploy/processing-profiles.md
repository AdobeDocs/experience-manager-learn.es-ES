---
title: Integración de los trabajadores de Asset Compute con Perfiles de procesamiento de AEM
description: AEM as a Cloud Service se integra con los trabajadores de Asset Compute implementados en Adobe I/O Runtime mediante Perfiles de procesamiento de AEM Assets. Los perfiles de procesamiento se configuran en el servicio Autor para procesar recursos específicos mediante trabajadores personalizados y almacenar los archivos generados por los trabajadores como representaciones de recursos.
feature: Microservicios de Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integraciones, desarrollo
role: Desarrollador
level: Intermedio, con experiencia
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 2%

---


# Integración con Perfiles de procesamiento AEM

Para que los trabajadores de Asset Compute generen representaciones personalizadas en AEM as a Cloud Service, deben registrarse en el servicio de autor de AEM as a Cloud Service a través de Perfiles de procesamiento. Todos los recursos sujetos a ese perfil de procesamiento tendrán el trabajador invocado al cargar o volver a procesar, y tendrán la representación personalizada generada y disponible a través de las representaciones del recurso.

## Definir un perfil de procesamiento

Primero cree un nuevo perfil de procesamiento que invoque al trabajador con los parámetros configurables.

![Perfil de procesamiento](./assets/processing-profiles/new-processing-profile.png)

1. Inicie sesión en AEM as a Cloud Service Author service como __administrador de AEM__. Como este es un tutorial, recomendamos utilizar un entorno de desarrollo o un entorno en un Simulador para pruebas.
1. Vaya a __Herramientas > Assets > Perfiles de procesamiento__
1. Toque el botón __Crear__
1. Asigne un nombre al perfil de procesamiento: `WKND Asset Renditions`
1. Pulse la pestaña __Personalizado__ y pulse __Agregar nuevo__
1. Definir el nuevo servicio
   + __Nombre de la representación:__ `Circle`
      + Representación de nombres de archivo que se utilizará para identificar esta representación en AEM Assets
   + __Extensión:__ `png`
      + Extensión de la representación que se generará. Se establece en `png` ya que este es el formato de salida admitido por el servicio web del trabajador y resulta en un fondo transparente detrás del círculo cortado.
   + __Punto final:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Esta es la dirección URL del trabajador obtenida mediante `aio app get-url`. Asegúrese de que la dirección URL señale en el espacio de trabajo correcto en función del entorno de AEM as a Cloud Service.
      + Asegúrese de que la URL de trabajo señale al espacio de trabajo correcto. AEM as a Cloud Service Stage debe utilizar la URL del espacio de trabajo de ensayo y AEM as a Cloud Service Production debe utilizar la URL del espacio de trabajo de producción.
   + __Parámetros de servicio__
      + Toque __Agregar parámetro__
         + Clave: `size`
         + Value: `1000`
      + Toque __Agregar parámetro__
         + Clave: `contrast`
         + Valor: `0.25`
      + Toque __Agregar parámetro__
         + Clave: `brightness`
         + Valor: `0.10`
      + Estos pares de clave/valor que se pasan al trabajador de Asset Compute y están disponibles a través del objeto JavaScript `rendition.instructions`.
   + __Tipos MIME__
      + __Incluye:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Estos tipos MIME son los únicos módulos npm del trabajador. Esta lista limita qué recursos procesará el trabajador personalizado.
      + __Excluye:__ `Leave blank`
         + Nunca procese recursos con estos tipos MIME con esta configuración de servicio. En este caso, solo utilizamos una lista de permitidos.
1. Toque __Guardar__ en la parte superior derecha

## Aplicar e invocar un perfil de procesamiento

1. Seleccione el perfil de procesamiento recién creado, `WKND Asset Renditions`
1. Toque __Aplicar perfil a las carpetas__ en la barra de acciones superior
1. Seleccione una carpeta a la que aplicar el perfil de procesamiento, como `WKND` y pulse __Aplicar__
1. Vaya a la carpeta a la que no se aplicó el perfil de procesamiento mediante __AEM > Assets > Archivos__ y pulse `WKND`.
1. Cargue algunos recursos de imágenes nuevos ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) y [sample-3.jpg](../assets/samples/sample-3.jpg)) en cualquier carpeta de la carpeta con el perfil de procesamiento aplicado y espere a que se procese el recurso cargado.
1. Pulse el recurso para abrir sus detalles
   + Las representaciones predeterminadas pueden generarse y aparecer más rápido en AEM que en las representaciones personalizadas.
1. Abra la vista __Representaciones__ en la barra lateral izquierda
1. Pulse en el recurso llamado `Circle.png` y revise la representación generada

   ![Representación generada](./assets/processing-profiles/rendition.png)

## Terminados!

Felicitaciones! Ha finalizado el [tutorial](../overview.md) sobre cómo ampliar los microservicios de AEM as a Cloud Service Asset Compute. Ahora debe tener la capacidad de configurar, desarrollar, probar, depurar e implementar trabajadores personalizados de Asset Compute para que los use su servicio de autor de AEM as a Cloud Service.

### Revisión del código fuente completo del proyecto en Github

El proyecto final de Asset Compute está disponible en Github en:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene es el estado final del proyecto, totalmente completado con los casos de trabajo y prueba, pero no contiene credenciales, por ejemplo. `.env`,  `.config.json` o  `.aio`._

## Solución de problemas

+ [Falta una representación personalizada en el recurso en AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [El procesamiento de recursos falla en AEM](../troubleshooting.md#asset-processing-fails)
