---
title: Integración de trabajadores de Asset compute AEM con perfiles de procesamiento de
description: AEM as a Cloud Service se integra con los trabajadores de Asset compute implementados en Adobe I/O Runtime mediante Perfiles de procesamiento de AEM Assets. Los perfiles de procesamiento se configuran en el servicio Autor para procesar recursos específicos mediante programas de trabajo personalizados y almacenar los archivos generados por los trabajadores como representaciones de recursos.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 126
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# AEM Integración con Perfiles de procesamiento de

Para que los trabajadores de Asset Compute puedan generar representaciones personalizadas en AEM as a Cloud Service, deben estar registrados en el servicio de AEM as a Cloud Service Author a través de Perfiles de procesamiento. Todos los recursos sujetos a ese perfil de procesamiento invocarán al trabajador en el momento de la carga o el reprocesamiento, y tendrán la representación personalizada generada y disponible a través de las representaciones del recurso.

## Definir un perfil de procesamiento

En primer lugar, cree un nuevo perfil de procesamiento que invoque al trabajador con los parámetros configurables.

![Perfil de procesamiento](./assets/processing-profiles/new-processing-profile.png)

1. Inicie sesión en el servicio de AEM as a Cloud Service AEM Author como __Administrador de la__. Como este es un tutorial, recomendamos utilizar un entorno de desarrollo o un entorno en una zona protegida.
1. Vaya a __Herramientas > Assets > Perfiles de procesamiento__
1. Pulse el botón __Crear__
1. Asigne un nombre al perfil de procesamiento, `WKND Asset Renditions`
1. Pulse la pestaña __Personalizar__ y luego pulse __Agregar nuevo__
1. Defina el nuevo servicio
   + __Nombre de representación:__ `Circle`
      + El nombre de archivo de la representación que se utilizó para identificar esta representación en AEM Assets
   + __Extensión:__ `png`
      + Extensión de la representación que se genera. Establezca el valor en `png`, ya que este es el formato de salida admitido por el servicio web del trabajador, y da como resultado un fondo transparente detrás del círculo cortado.
   + __Punto final:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Dirección URL del trabajador obtenida mediante `aio app get-url`. Asegúrese de que la dirección URL apunta al espacio de trabajo correcto en función del entorno de AEM as a Cloud Service.
      + Asegúrese de que la dirección URL del trabajador apunta al espacio de trabajo correcto. AEM as a Cloud Service Stage debe utilizar la URL del espacio de trabajo de fase y AEM as a Cloud Service Production debe utilizar la URL del espacio de trabajo de producción.
   + __Parámetros de servicio__
      + Pulse __Agregar parámetro__
         + Clave: `size`
         + Valor: `1000`
      + Pulse __Agregar parámetro__
         + Clave: `contrast`
         + Valor: `0.25`
      + Pulse __Agregar parámetro__
         + Clave: `brightness`
         + Valor: `0.10`
      + Estos pares clave/valor que se pasan al trabajador de Asset compute y que están disponibles a través del objeto JavaScript `rendition.instructions`.
   + __Tipos MIME__
      + __Incluye:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Estos tipos MIME son los únicos en los módulos npm del trabajador. Esta lista limita los recursos que procesa el trabajador personalizado.
      + __Exclusiones:__ `Leave blank`
         + Nunca procese recursos con estos tipos MIME mediante esta configuración de servicio. En este caso, solo se utiliza una lista de permitidos.
1. Pulse __Guardar__ en la parte superior derecha

## Aplicar e invocar un perfil de procesamiento

1. Seleccione el perfil de procesamiento recién creado, `WKND Asset Renditions`
1. Pulse __Aplicar perfil a las carpetas__ en la barra de acciones superior
1. Seleccione una carpeta a la que aplicar el perfil de procesamiento, como `WKND`, y pulse __Aplicar__
1. AEM Vaya a la carpeta a la que no se aplicó el perfil de procesamiento mediante __> Assets > Archivos__ y pulse `WKND`.
1. Cargue algunos recursos de imágenes nuevas ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) y [sample-3.jpg](../assets/samples/sample-3.jpg)) en cualquier carpeta de la carpeta con el perfil de procesamiento aplicado y espere a que se procese el recurso cargado.
1. Pulse el recurso para abrir sus detalles
   + AEM Las representaciones predeterminadas pueden generarse y aparecer con mayor rapidez en que las representaciones personalizadas.
1. Abra la vista __Representaciones__ desde la barra lateral izquierda
1. Pulse en el recurso llamado `Circle.png` y revise la representación generada

   ![Representación generada](./assets/processing-profiles/rendition.png)

## ¡Terminado!

Enhorabuena. ¡Ha finalizado el [tutorial](../overview.md) sobre cómo extender los microservicios de Asset compute de AEM as a Cloud Service! Ahora debe tener la capacidad de configurar, desarrollar, probar, depurar e implementar los trabajadores de Asset compute personalizados para su servicio de AEM as a Cloud Service Author.

### Revise el código fuente completo del proyecto en Github

El proyecto de Asset compute final está disponible en Github en:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contiene es el estado final del proyecto, totalmente relleno con los casos de prueba y trabajo, pero no contiene credenciales, es decir. `.env`, `.config.json` o `.aio`._

## Resolución de problemas

+ [AEM Falta la representación personalizada en el recurso en el recurso en el que se ha realizado el](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM El procesamiento de recursos falla en la](../troubleshooting.md#asset-processing-fails)
