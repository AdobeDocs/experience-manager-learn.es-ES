---
title: AEM Registros comunes de Dispatcher
description: Eche un vistazo a las entradas de registro comunes de Dispatcher y aprenda qué significan y cómo solucionarlas.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 252
source-git-commit: 80c04ce1ad7d60c1fc75ecc194dd54a2ad5b82fa
workflow-type: tm+mt
source-wordcount: '796'
ht-degree: 0%

---

# Registros comunes

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: URL de vanidad](./disp-vanity-url.md)

## Información general

El documento describe las entradas de registro comunes que verá, qué significan y cómo gestionarlas.

## Advertencia de GLOB

Ejemplo de entrada de registro:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Desde el módulo 4.2.x de Dispatcher, no se recomienda utilizar los siguientes tipos de coincidencias en el archivo de filtros:

```
/0041 { /type "allow" /glob "* *.css *"   }
```

En su lugar, es mejor utilizar una nueva sintaxis como:

```
/0041 { /type "allow" /url "*.css" }
```

O incluso mejor que no coincida con un elemento de coincidencia comodín:

```
/0041 { /type "allow" /extension "css" }
```

Al utilizar cualquiera de los métodos sugeridos se elimina ese mensaje de error de los registros.

## Filtrar rechazos

>[!NOTE]
>
>Estas entradas no siempre aparecen aunque se produzcan rechazos si el nivel de registro es demasiado bajo. Configúrelo como Información o Depurar para asegurarse de que los filtros rechacen las solicitudes.

Ejemplo de entrada de registro:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

o:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

>[!CAUTION]
>
>Comprenda que las reglas de Dispatcher se establecieron para filtrar esa solicitud. En este caso, la página que se intentó visitar se rechazó a propósito y no queremos hacer nada al respecto.

Si el registro tiene el siguiente aspecto:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Eso nos permite saber que nuestro archivo de diseño `.eot` se está bloqueando y queremos solucionarlo.
Por lo tanto, deberíamos mirar nuestro archivo de filtros y añadir la siguiente línea para permitir `.eot` archivos mediante

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

Esto permite que se utilice el archivo y evitaría que se registrara.
Si desea ver qué se está filtrando, ejecute este comando en el archivo de registro:

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Tiempos de espera del procesamiento

Ejemplo de entrada de registro de tiempo de espera de socket:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Esto ocurre cuando tiene la dirección IP incorrecta configurada en la sección de procesamiento de la granja. AEM Esa instancia o la instancia de dejaron de responder o escuchar y Dispatcher no puede acceder a ella.

AEM Compruebe las reglas del cortafuegos y que la instancia de la se esté ejecutando y en buen estado.

Ejemplo de entradas de registro de tiempo de espera de puerta:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

AEM Esto significa que la instancia de tenía un socket abierto al que se podía acceder y se agotó el tiempo de espera para la respuesta. AEM Esto significa que la instancia de era demasiado lenta o no estaba en buen estado y Dispatcher alcanzó el tiempo de espera establecido en la sección de procesamiento de la granja. AEM Aumente la configuración de tiempo de espera o arregle la instancia de la.

## Nivel de almacenamiento en caché

Ejemplo de entrada de registro:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Esto significa que se mide la recuperación desde el nivel de procesamiento frente a la caché. Desea obtener más del 80 % de aciertos de la caché y debe seguir las instrucciones de la ayuda [aquí](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html?lang=es):

Para que este número sea lo más alto posible.

>[!NOTE]
>
>Incluso si tiene la configuración de la caché en el archivo de granja de servidores para almacenar en caché todo lo que se vacíe con demasiada frecuencia o de forma agresiva, lo que puede provocar que se produzca un porcentaje menor de aciertos de caché

## Falta el directorio

Ejemplo de entrada de registro:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

Esto generalmente aparece cuando se busca un elemento mientras se realiza una limpieza de la caché al mismo tiempo.

También es posible que el directorio raíz del documento tenga permisos incorrectos o que el contexto de archivo SELinux no sea el correcto en la carpeta que impide que apache cree los nuevos subdirectorios necesarios.

Para problemas de permisos, consulte los permisos de la raíz del documento y asegúrese de que tengan un aspecto similar al siguiente:

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## URL de vanidad no encontrada

Ejemplo de entrada de registro:

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

AEM Este error se produce cuando se configura Dispatcher para que utilice el filtro automático dinámico para admitir direcciones URL mnemónicas, pero no se ha finalizado la configuración instalando el paquete en el procesador de.

AEM Para solucionarlo, instale el paquete de funciones de la URL mnemónica en la instancia de y permita que lo pueda preparar un usuario anónimo. Detalles [aquí](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html)

La configuración de una URL de vanidad en funcionamiento es la siguiente:

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

Este error indica que de todos los archivos de granja disponibles en `/etc/httpd/conf.dispatcher.d/enabled_farms/` no han podido encontrar una entrada coincidente de la `/virtualhost` sección.

Los archivos de granja de servidores coinciden con el tráfico en función del nombre de dominio o la ruta incluidos en la solicitud. Utiliza la coincidencia GLOB y, si no coincide, la configuración de la granja no es correcta, está mal escrita en la entrada de la granja o no se ha incluido. Cuando el conjunto de servidores no coincide con ninguna entrada, se establece de forma predeterminada la última granja de servidores incluida en la pila de archivos de la granja de servidores incluidos. En este ejemplo era `999_ams_publish_farm.any` que recibe el nombre genérico de publishfarm.

Este es un ejemplo de archivo de granja de servidores `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` que se ha reducido para resaltar las partes relevantes.

## Elemento obtenido desde

Ejemplo de entrada de registro:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

La página se obtuvo mediante el método GET http para el contenido `/content/we-retail/us/en.html` y tomó 24034 milisegundos. La parte a la que queremos prestar atención está al final `publishfarm/0`. Verá que se ha dirigido a y que coincide con el `publishfarm`. La solicitud se obtuvo del procesamiento 0. AEM Esto significa que se tuvo que solicitar la página a los usuarios y, a continuación, se almacenó en caché la. Vamos a volver a solicitar esta página para ver qué sucede con el registro.

[Siguiente -> Archivos de solo lectura](./immutable-files.md)
