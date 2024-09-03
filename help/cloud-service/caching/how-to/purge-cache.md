---
title: Depuración de la caché de CDN
description: Obtenga información sobre cómo depurar o eliminar la respuesta HTTP en caché de la CDN de AEM as a Cloud Service.
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-13T00:00:00Z
jira: KT-15963
thumbnail: KT-15963.jpeg
exl-id: 5d81f6ee-a7df-470f-84b9-12374c878a1b
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '892'
ht-degree: 0%

---

# Depuración de la caché de CDN

Obtenga información sobre cómo depurar o eliminar la respuesta HTTP en caché de la CDN de AEM as a Cloud Service. Mediante la característica de autoservicio **Purgar token de API**, puede purgar la caché de un recurso específico, un grupo de recursos y toda la caché.

AEM En este tutorial, aprenderá a configurar y utilizar el token de API de purga para purgar la caché de CDN del sitio de muestra [WKND](https://github.com/adobe/aem-guides-wknd) mediante la función de autoservicio.

>[!VIDEO](https://video.tv.adobe.com/v/3432948?quality=12&learn=on)

## Invalidación de caché frente a purga explícita

Existen dos formas de eliminar los recursos en caché de la CDN:

1. **Invalidación de caché:** Es el proceso de quitar los recursos en caché de la red de distribución de contenido (CDN) en función de encabezados de caché como `Cache-Control`, `Surrogate-Control` o `Expires`. El valor de atributo `max-age` del encabezado de la caché se usa para determinar la duración de la caché de los recursos, también conocida como TTL (Tiempo de vida) de la caché. Cuando caduca la duración de la caché, los recursos en caché se eliminan automáticamente de la caché de CDN.

1. **Depuración explícita:** Es el proceso de quitar manualmente los recursos en caché de la caché de CDN antes de que caduque el TTL. La depuración explícita resulta útil cuando desea eliminar los recursos en caché inmediatamente. Sin embargo, aumenta el tráfico al servidor de origen.

Cuando los recursos en caché se eliminan de la caché de CDN, la siguiente solicitud para el mismo recurso obtiene la última versión del servidor de origen.

## Configuración del token de API de depuración

Vamos a aprender a configurar el token de API de purga para purgar la caché de CDN.

### Configuración de la regla de CDN

AEM El token de API de purga se crea configurando la regla de CDN en el código de proyecto de.

1. AEM Abra el archivo `cdn.yaml` desde la carpeta principal `config` de su proyecto de la. Por ejemplo, el archivo cdn.yaml](https://github.com/adobe/aem-guides-wknd/blob/main/config/cdn.yaml) del proyecto [WKND.

1. Agregue la siguiente regla de CDN al archivo `cdn.yaml`:

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:  
  authentication: # The main authentication configuration
    authenticators: # The list of authenticators
       - name: purge-auth # The name of the authenticator
         type: purge  # The type of the authenticator, must be purge
         purgeKey1: ${{CDN_PURGEKEY_081324}} # The first purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_073124}}
         purgeKey2: ${{CDN_PURGEKEY_111324}} # The second purge key, must be referenced by the Cloud Manager secret-type environment variable name ${{CDN_EDGEKEY_111324}}. It is used for the rotation of secrets without any interruptions.
    rules: # The list of authentication rules
       - name: purge-auth-rule # The name of the rule
         when: { reqProperty: tier, equals: "publish" } # The condition when the rule should be applied
         action: # The action to be taken when the rule is applied
           type: authenticate # The type of the action, must be authenticate
           authenticator: purge-auth # The name of the authenticator to be used, must match the name from the above authenticators list               
```

1. Guarde, confirme e inserte los cambios en el repositorio de flujo ascendente de Adobe.

### Crear variable de entorno de Cloud Manager

A continuación, cree las variables de entorno de Cloud Manager para almacenar el valor Purgar token de API.

1. Inicie sesión en Cloud Manager en [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) y seleccione su organización y programa.

1. En la sección __Entornos__, haga clic en los **puntos suspensivos** (...) junto al entorno deseado y seleccione **Ver detalles**.

   ![Ver detalles](../assets/how-to/view-env-details.png)

1. A continuación, seleccione la ficha **Configuración** y haga clic en el botón **Agregar configuración**.

1. En el cuadro de diálogo **Configuración del entorno**, escriba los siguientes detalles:
   - **Nombre**: escriba el nombre de la variable de entorno. Debe coincidir con el valor `purgeKey1` o `purgeKey2` del archivo `cdn.yaml`.
   - **Valor**: escriba el valor del token de API de purga.
   - **Servicio aplicado**: seleccione la opción **Todos**.
   - **Tipo**: seleccione la opción **Secreto**.
   - Haga clic en el botón **Agregar**.

   ![Agregar variable](../assets/how-to/add-cloud-manager-secrete-variable.png)

1. Repita los pasos anteriores para crear la segunda variable de entorno para el valor `purgeKey2`.

1. Haga clic en **Guardar** para guardar y aplicar los cambios.

### Implementación de la regla CDN

Finalmente, implemente la regla de CDN configurada en el entorno de AEM as a Cloud Service mediante la canalización de Cloud Manager.

1. En Cloud Manager, vaya a la sección **Canalizaciones**.

1. Cree una nueva canalización o seleccione la canalización existente que implemente solamente los archivos **Config**. Para ver los pasos detallados, consulte [Crear una canalización de configuración](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

1. Haga clic en el botón **Ejecutar** para implementar la regla de CDN.

   ![Ejecutar canalización](../assets/how-to/run-config-pipeline.png)

## Uso del token de API de purga

AEM Para purgar la caché de la CDN, invoque la URL de dominio específica del servicio de con Purge API Token. La sintaxis para purgar la caché es la siguiente:

```
PURGE <URL> HTTP/1.1
Host: <AEM_SERVICE_SPECIFIC_DOMAIN>
X-AEM-Purge-Key: <PURGE_API_TOKEN>
X-AEM-Purge: <PURGE_TYPE>
Surrogate-Key: <SURROGATE_KEY>
```

Donde:

- **PURGE`<URL>`**: al método `PURGE` le sigue la ruta de acceso URL del recurso que desea purgar.
- AEM **Host:`<AEM_SERVICE_SPECIFIC_DOMAIN>`**: Especifica el dominio del servicio de la.
- AEM **X--Purge-Key:`<PURGE_API_TOKEN>`**: Un encabezado personalizado que contiene el valor Purge API Token.
- AEM **X--Purge:`<PURGE_TYPE>`**: un encabezado personalizado que especifica el tipo de operación de depuración. El valor puede ser `hard`, `soft` o `all`. En la tabla siguiente se describe cada tipo de depuración:

  | Tipo de purga | Descripción |
  |:------------:|:-------------:|
  | duro (predeterminado) | Quita el recurso almacenado en caché inmediatamente. Evite esta acción, ya que aumenta el tráfico hacia el servidor de origen. |
  | blando | Marca el recurso almacenado en caché como obsoleto y obtiene la última versión del servidor de origen. |
  | todo | Quita todos los recursos en caché de la caché de CDN. |

- **Clave de sustitución:`<SURROGATE_KEY>`**: (opcional) un encabezado personalizado que especifica las claves de sustitución (separadas por espacio) de los grupos de recursos que se van a purgar. La clave de sustitución se utiliza para agrupar los recursos y debe configurarse en el encabezado de respuesta del recurso.

>[!TIP]
>
>En los ejemplos siguientes, `X-AEM-Purge: hard` se usa con fines de demostración. Puede reemplazarlo con `soft` o `all` según sus necesidades. Tenga cuidado al usar el tipo de depuración `hard`, ya que aumenta el tráfico al servidor de origen.

### Purgar la caché de un recurso específico

En este ejemplo, el comando `curl` purga la caché del recurso `/us/en.html` en el sitio WKND implementado en un entorno AEM as a Cloud Service.

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/us/en.html" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

Una vez realizada la purga correctamente, se devuelve una respuesta `200 OK` con contenido JSON.

```json
{ "status": "ok", "id": "1000098-1722961031-13237063" }
```

### Purga de la caché para un grupo de recursos

En este ejemplo, el comando `curl` purga la caché del grupo de recursos con la clave suplente `wknd-assets`. El encabezado de respuesta `Surrogate-Key` está establecido en [`wknd.vhost`](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L176), por ejemplo:

```http
<VirtualHost *:80>
    ...

    # Core Component Image Component: long-term caching (30 days) for immutable URLs, background refresh to avoid MISS
    <LocationMatch "^/content/.*\.coreimg.*\.(?i:jpe?g|png|gif|svg)$">
        Header set Cache-Control "max-age=2592000,stale-while-revalidate=43200,stale-if-error=43200,public,immutable" "expr=%{REQUEST_STATUS} < 400"
        # Set Surrogate-Key header to group the cache of WKND assets, thus it can be flushed independtly
        Header set Surrogate-Key "wknd-assets"
        Header set Age 0
    </LocationMatch>

    ...
</VirtualHost>
```

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com" \
-H "Surrogate-Key: wknd-assets" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: hard"
```

Una vez realizada la purga correctamente, se devuelve una respuesta `200 OK` con contenido JSON.

```json
{ "wknd-assets": "10027-1723478994-2597809-1" }
```

### Purgar toda la caché

En este ejemplo, con el comando `curl`, toda la caché se purga del sitio WKND de ejemplo implementado en el entorno de AEM as a Cloud Service.

```bash
curl -X PURGE "https://publish-p46652-e1315806.adobeaemcloud.com/" \
-H "X-AEM-Purge-Key: 123456789" \
-H "X-AEM-Purge: all"
```

Una vez realizada la purga correctamente, se devuelve una respuesta `200 OK` con contenido JSON.

```json
{"status":"ok"}
```

### Verificar la depuración de caché

Para verificar la depuración de caché, acceda a la URL del recurso en el explorador web y revise los encabezados de respuesta. El valor del encabezado `X-Cache` debe ser `MISS`.

![Encabezado De X-Cache](../assets/how-to/x-cache-miss.png)
