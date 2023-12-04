---
title: Integración de trabajadores de Asset compute AEM con perfiles de procesamiento de
description: AEM El as a Cloud Service se integra con los trabajadores de Asset compute implementados en Adobe I/O Runtime a través de los perfiles de procesamiento de AEM Assets. Los perfiles de procesamiento se configuran en el servicio Autor para procesar recursos específicos mediante programas de trabajo personalizados y almacenar los archivos generados por los trabajadores como representaciones de recursos.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 179
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# AEM Integración con Perfiles de procesamiento de

Para que los Asset compute AEM de trabajo generen representaciones personalizadas en el as a Cloud Service AEM de la, deben estar registrados en el servicio de creación as a Cloud Service de a través de Perfiles de procesamiento. Todos los recursos sujetos a ese perfil de procesamiento invocarán al trabajador en el momento de la carga o el reprocesamiento, y tendrán la representación personalizada generada y disponible a través de las representaciones del recurso.

## Definir un perfil de procesamiento

En primer lugar, cree un nuevo perfil de procesamiento que invoque al trabajador con los parámetros configurables.

![Perfil de procesamiento](./assets/processing-profiles/new-processing-profile.png)

1. AEM Inicie sesión en el servicio de autor as a Cloud Service de como __AEM Administrador de__. Como este es un tutorial, recomendamos utilizar un entorno de desarrollo o un entorno en una zona protegida.
1. Vaya a __Herramientas > Recursos > Perfiles de procesamiento__
1. Tocar __Crear__ botón
1. Asigne un nombre al perfil de procesamiento. `WKND Asset Renditions`
1. Pulse el botón __Personalizado__ y pulse __Añadir nuevo__
1. Defina el nuevo servicio
   + __Nombre de representación:__ `Circle`
      + El nombre de archivo de la representación que se utilizó para identificar esta representación en AEM Assets
   + __Extensión:__ `png`
      + Extensión de la representación que se genera. Configure como. `png` como este es el formato de salida admitido por el servicio web del trabajador, y da como resultado un fondo transparente detrás del círculo cortado.
   + __Punto final:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dirección URL del trabajador obtenida mediante `aio app get-url`. AEM Asegúrese de que la dirección URL apunta al espacio de trabajo correcto en función del entorno as a Cloud Service de la.
      + Asegúrese de que la dirección URL del trabajador apunta al espacio de trabajo correcto. AEM La fase as a Cloud Service AEM debe utilizar la URL del espacio de trabajo de fase, y la producción as a Cloud Service debe utilizar la URL del espacio de trabajo de producción de.
   + __Parámetros de servicio__
      + Tocar __Añadir parámetro__
         + Clave: `size`
         + Valor: `1000`
      + Tocar __Añadir parámetro__
         + Clave: `contrast`
         + Valor: `0.25`
      + Tocar __Añadir parámetro__
         + Clave: `brightness`
         + Valor: `0.10`
      + Estos pares clave/valor que se pasan al trabajador de Asset compute y que están disponibles a través de `rendition.instructions` Objeto JavaScript.
   + __Tipos MIME__
      + __Incluye:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Estos tipos MIME son los únicos en los módulos npm del trabajador. Esta lista limita los recursos que procesa el trabajador personalizado.
      + __Excluye:__ `Leave blank`
         + Nunca procese recursos con estos tipos MIME mediante esta configuración de servicio. En este caso, solo se utiliza una lista de permitidos.
1. Tocar __Guardar__ en la parte superior derecha

## Aplicar e invocar un perfil de procesamiento

1. Seleccione el perfil de procesamiento recién creado, `WKND Asset Renditions`
1. Tocar __Aplicar perfil a las carpetas__ en la barra de acciones superior
1. Seleccione una carpeta a la que aplicar el perfil de procesamiento, como `WKND` y pulse __Aplicar__
1. Vaya a la carpeta en la que no se aplicó el perfil de procesamiento mediante __AEM > Recursos > Archivos__ y acceder a `WKND`.
1. Cargar algunos recursos de imágenes nuevos ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg), y [sample-3.jpg](../assets/samples/sample-3.jpg)) en cualquier carpeta de la carpeta con el perfil de procesamiento aplicado y espere a que se procese el recurso cargado.
1. Pulse el recurso para abrir sus detalles
   + AEM Las representaciones predeterminadas pueden generarse y aparecer con mayor rapidez en que las representaciones personalizadas.
1. Abra el __Representaciones__ vista desde la barra lateral izquierda
1. Pulse en el recurso llamado `Circle.png` y revise la representación generada

   ![Representación generada](./assets/processing-profiles/rendition.png)

## ¡Terminado!

Felicitaciones. Ha finalizado el [tutorial](../overview.md) AEM sobre cómo extender los microservicios de Asset compute as a Cloud Service de la! Ahora debe tener la capacidad de configurar, desarrollar, probar, depurar e implementar Asset compute AEM personalizadas para que las use el servicio de autor as a Cloud Service de la.

### Revise el código fuente completo del proyecto en Github

El proyecto de Asset compute final está disponible en Github en:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene es el estado final del proyecto, completamente relleno con los casos de trabajo y prueba, pero no contiene credenciales, es decir. `.env`, `.config.json` o `.aio`._

## Solución de problemas

+ [AEM Falta la representación personalizada en el recurso en el recurso en el que se ha realizado el](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM El procesamiento de recursos falla en la](../troubleshooting.md#asset-processing-fails)
