---
title: Integrar a los trabajadores de Asset compute con Perfiles de procesamiento de AEM
description: AEM como Cloud Service se integra con los trabajadores de Asset compute desplegados en Adobe I/O Runtime mediante Perfiles de procesamiento de AEM Assets. Los Perfiles de procesamiento se configuran en el servicio Autor para procesar recursos específicos mediante programas de trabajo personalizados y almacenar los archivos generados por los trabajadores como representaciones de recursos.
feature: asset-compute, processing-profiles
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 2%

---


# Integración con Perfiles de procesamiento de AEM

Para que los trabajadores de Asset compute generen representaciones personalizadas en AEM como Cloud Service, deben registrarse en AEM como un servicio de creación Cloud Service a través de Perfiles de procesamiento. Todos los recursos sujetos a ese Perfil de procesamiento tendrán el programa de trabajo invocado al cargar o volver a procesar, y la representación personalizada se generará y estará disponible mediante las representaciones del recurso.

## Definir un Perfil de procesamiento

En primer lugar, cree un nuevo Perfil de procesamiento que invoque al programa de trabajo con los parámetros configurables.

![Perfil de procesamiento](./assets/processing-profiles/new-processing-profile.png)

1. Inicie sesión en AEM como un servicio Cloud Service Author como __AEM administrador__. Como este es un tutorial, recomendamos el uso de un entorno Dev o un entorno en un Simulador para pruebas.
1. Vaya a __Herramientas > Recursos > Perfiles de procesamiento__
1. Tocar el botón __Crear__
1. Asigne un nombre al Perfil de procesamiento: `WKND Asset Renditions`
1. Toque la ficha __Personalizado__ y toque __Añadir nuevo__
1. Definir el nuevo servicio
   + __Nombre de representación:__ `Circle`
      + Representación del nombre de archivo que se utilizará para identificar esta representación en AEM Assets
   + __Extensión:__ `png`
      + La extensión de la representación que se generará. Se establece en `png` ya que es el formato de salida admitido por el servicio Web del trabajador y resulta en un fondo transparente detrás del círculo cortado.
   + __Extremo:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Es la dirección URL del trabajador obtenida mediante `aio app get-url`. Asegúrese de que la dirección URL apunte al espacio de trabajo correcto según la AEM como entorno de Cloud Service.
      + Asegúrese de que la dirección URL del trabajo señala al espacio de trabajo correcto. AEM como etapa de Cloud Service debe utilizar la dirección URL del espacio de trabajo de la fase y AEM como Cloud Service Producción debe utilizar la dirección URL del espacio de trabajo Producción.
   + __Parámetros de servicio__
      + Toque __Añadir parámetro__
         + Clave: `size`
         + Value: `1000`
      + Toque __Añadir parámetro__
         + Clave: `contrast`
         + Valor: `0.25`
      + Toque __Añadir parámetro__
         + Clave: `brightness`
         + Valor: `0.10`
      + Estos pares de clave y valor que se pasan al programa de trabajo de Asset compute y están disponibles a través del objeto JavaScript `rendition.instructions`.
   + __Tipos MIME__
      + __Incluye:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Estos tipos MIME son los únicos módulos npm del trabajador. Esta lista limita los recursos que procesará el programa de trabajo personalizado.
      + __Excluye:__ `Leave blank`
         + Nunca procese recursos con estos tipos MIME mediante esta configuración de servicio. En este caso, sólo usamos una lista de permitidos.
1. Toque __Guardar__ en la parte superior derecha

## Aplicar e invocar un Perfil de procesamiento

1. Seleccione el Perfil de procesamiento recién creado, `WKND Asset Renditions`
1. Toque __Aplicar Perfil a las carpetas__ en la barra de acciones superior
1. Seleccione una carpeta a la que aplicar el Perfil de procesamiento, como `WKND` y toque __Aplicar__
1. Vaya a la carpeta a la que no se aplicó el Perfil de procesamiento mediante __AEM > Recursos > Archivos__ y toque `WKND`.
1. Cargue algunos recursos de imágenes nuevos ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) y [sample-3.jpg](../assets/samples/sample-3.jpg)) en cualquier carpeta de la carpeta con el Perfil de procesamiento aplicado y espere a que se procese el recurso cargado.
1. Toque el recurso para abrir sus detalles
   + Las representaciones predeterminadas pueden generarse y aparecer más rápidamente en AEM que las representaciones personalizadas.
1. Abra la vista __Representaciones__ desde la barra lateral izquierda
1. Toque en el recurso denominado `Circle.png` y revise la representación generada

   ![Representación generada](./assets/processing-profiles/rendition.png)

## Terminados!

Felicitaciones! Ha terminado el [tutorial](../overview.md) sobre cómo extender AEM como microservicios de Asset compute de Cloud Service! Ahora debe tener la capacidad de configurar, desarrollar, probar, depurar e implementar trabajadores de Asset compute personalizados para que los use su AEM como servicio de creación de Cloud Service.

### Revise el código fuente completo del proyecto en Github

El proyecto de Asset compute final está disponible en Github en:

+ [aem-guide-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene es el estado final del proyecto, totalmente rellenado con los casos de trabajo y prueba, pero no contiene credenciales, por ejemplo. `.env`,  `.config.json` o  `.aio`._

## Solución de problemas

+ [Falta la representación personalizada del recurso en AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [El procesamiento de recursos falla en AEM](../troubleshooting.md#asset-processing-fails)
