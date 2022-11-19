---
title: Registros comunes de AEM Dispatcher
description: Eche un vistazo a las entradas de registro comunes de Dispatcher y aprenda qué significan y cómo abordarlas.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '812'
ht-degree: 0%

---


# Registros comunes

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: URL mnemónicas](./disp-vanity-url.md)

## Información general

El documento describirá las entradas de registro comunes que verá, lo que significan y cómo tratarlas.

## Advertencia de GLOB

Ejemplo de entrada de registro:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Desde el módulo 4.2.x de Dispatcher, han empezado a desalentar a las personas para que utilicen el siguiente tipo de coincidencias en su archivo de filtros:

```
/0041 { /type "allow" /glob "* *.css *"   }
```

En su lugar, es mejor utilizar la nueva sintaxis como:

```
/0041 { /type "allow" /url "*.css" }
```

O incluso mejor que no coincida con un coincidente comodín:

```
/0041 { /type "allow" /extension "css" }
```

Al realizar cualquiera de los métodos sugeridos, se eliminaría ese mensaje de error de los registros.

## Rechazos de filtro


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Estas entradas no siempre aparecen aunque se produzcan rechazos si el nivel de registro es demasiado bajo. Configúrelo en Información o Depuración para asegurarse de que los filtros rechacen las solicitudes.
</div>

Ejemplo de entrada de registro:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

o:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>Precaución:</b>

Comprenda que las reglas de Dispatcher se establecieron para filtrar esa solicitud. En este caso, la página que se intentó visitar fue rechazada a propósito y no quisiéramos hacer nada con esto.
</div>

Si su registro tiene el siguiente aspecto:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Eso nos hace saber que nuestro archivo de diseño `.eot` está siendo bloqueado y queremos arreglarlo.
Por lo tanto, deberíamos mirar nuestro archivo de filtros y agregar la siguiente línea para permitir `.eot` archivos hasta

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

Esto permitiría que el archivo pasara y evitaría que se registrara.
Si desea ver qué se está filtrando, puede ejecutar este comando en el archivo de registro:

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Tiempos de espera del procesamiento

Entrada de registro de muestra de tiempo de espera de socket:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Esto ocurre cuando tiene la dirección IP incorrecta configurada en la sección de renderizaciones de su granja. Que o la instancia de AEM dejó de responder o escuchar y Dispatcher no puede acceder a ella.

Compruebe las reglas del firewall y que la instancia de AEM se esté ejecutando y esté en buen estado.

Entradas de registro de muestra de tiempo de espera de puerta de enlace:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

Esto significa que la instancia de AEM tenía un socket abierto al que podía acceder y cuyo tiempo de espera se agotó con la respuesta. Esto significa que la instancia de AEM era demasiado lenta o no estaba en buen estado y Dispatcher alcanzó su configuración de tiempo de espera en la sección de procesamiento de la granja. Aumente la configuración de tiempo de espera o asegúrese de que la instancia de AEM esté en buen estado.

## Nivel de almacenamiento en caché

Ejemplo de entrada de registro:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Esto significa que se mide la recuperación desde el nivel de procesamiento frente a la caché. Desea obtener más del 80 % de visitas desde la caché y debe seguir la ayuda de [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

Para obtener este número lo más alto posible.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Incluso si tiene la configuración de caché en el archivo de granja para almacenar en caché todo lo que puede vaciar con demasiada frecuencia o de manera agresiva, lo que puede provocar que se produzca un porcentaje menor de aciertos de caché
</div>

## Falta el directorio

Ejemplo de entrada de registro:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

Esto suele aparecer cuando se busca un elemento mientras se realiza una limpieza de caché al mismo tiempo.

Ese o el directorio raíz del documento tiene permisos incorrectos en él o el contexto de archivo SELinux incorrecto en la carpeta que impide que apache cree los nuevos subdirectorios necesarios.

Para problemas de permisos, consulte los permisos de documentroot y asegúrese de que tengan un aspecto similar a:

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## No se ha encontrado la URL de vanidad

Ejemplo de entrada de registro:

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

Este error se produce cuando ha configurado el Dispatcher para que utilice el filtro automático dinámico para permitir direcciones URL mnemónicas, pero no ha finalizado la configuración instalando el paquete en el procesador de AEM.

Para arreglarlo, instale el paquete de funciones de la URL de vanidad en la instancia de AEM y permita que lo prepare el usuario anónimo. Detalles [here](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

Una configuración de URL de vanidad en funcionamiento tiene este aspecto:

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## Falta la granja

Ejemplo de entrada de registro:

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

Este error indica que de todos los archivos de granja disponibles en `/etc/httpd/conf.dispatcher.d/enabled_farms/` no pudieron encontrar una entrada coincidente del `/virtualhost` para obtener más información.

Los archivos de granja coinciden con el tráfico en función del nombre de dominio o la ruta con la que entró la solicitud. Utiliza la coincidencia global y, si no coincide, no ha configurado correctamente la granja, ha marcado la entrada en la granja o la entrada falta por completo. Cuando el conjunto de servidores no coincide con ninguna entrada, solo se establece de forma predeterminada el último conjunto de servidores incluido en la pila de archivos de granja incluidos. En este ejemplo, era `999_ams_publish_farm.any` que se denomina nombre genérico de publishfarm.

Este es un ejemplo de archivo de granja `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` se ha reducido para resaltar las partes relevantes.

## Elemento obtenido de

Ejemplo de entrada de registro:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

La página se obtuvo mediante el método http de GET para el contenido `/content/we-retail/us/en.html` y tomó 24034 milisegundos. La parte a la que queremos prestar atención es al final `publishfarm/0`. Verá que se ha dirigido a y que coincide con el `publishfarm`. La solicitud se obtuvo del render 0. Lo que significa que esta página tuvo que solicitarse desde AEM caché. Ahora vamos a solicitar esta página de nuevo y ver qué sucede con el registro.

[Siguiente -> Archivos de solo lectura](./immutable-files.md)