---
title: ¿Qué es Dispatcher?
description: Comprender lo que es un Dispatcher en realidad.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 829ad9733b4326c79b9b574b13b1d4c691abf877
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---


# ¿Qué es Dispatcher?

[Tabla de contenidos](./overview.md)

Comenzando con la descripción básica de lo que implica un Dispatcher AEM.

## Servidor web Apache

Comience con una instalación básica de Apache Web Server en un servidor Linux.

Explicación básica de lo que hace un servidor Apache:

- Sigue reglas simples para servir archivos a través de los protocolos HTTP(s) desde su directorio de documentos estático (`DocumentRoot`)
- Archivos almacenados en una ubicación predeterminada (`/var/www/html`) coinciden en las solicitudes y se procesan en el explorador del cliente solicitante




## AEM archivo de módulo específico (`mod_dispatcher.so`)

A continuación, agregue un complemento al servidor web Apache llamado módulo Dispatcher

Explicación básica de lo que hace el módulo Adobe AEM Dispatcher:

- Aumenta el controlador de archivos predeterminado
- Filtra las solicitudes incorrectas/Protege AEM vientre blando/extremos
- Balance de carga si hay más de un renderizador presente
- Permite un directorio de caché activo / Admite vaciado de archivos estancados
- Es la puerta principal de todas las instalaciones de AMS y entrega sitios web y activos al navegador del cliente
- Almacena en caché las solicitudes para volver a servir a una velocidad mucho más rápida de lo que un servidor AEM podría lograr por sí solo
- Mucho más...

## Flujo de trabajo del tráfico web

Comprender qué piezas se instalan juntas para construir un servidor Dispatcher básico nos lleva a que entienda el flujo de trabajo de tráfico web básico para una configuración de Servicios de Adobe Manager.
Esto le ayudará a comprender la función que desempeña en la cadena de sistemas que sirven contenido a los visitantes del contenido de AEM.

<b>Servir contenido ya almacenado en caché</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Ofrecer contenido nuevo de AEM</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if NOT found 
                → requests content from publisher 
                    → publisher sends content 
                        → Dispatcher adds content to cache and replies 
                            → End User
```

<b>Publicación de contenido/cambios</b>

```
AEM Author User activates content 
    → Triggers content to be replicated to Publisher 
        → Publisher gets content and triggers the flush request to Dispatcher 
            → Dispatcher invalidates changed content 
            * Next request for that content will request fresh copy from publisher *
```

[Siguiente -> Diseño de archivo básico](./basic-file-layout.md)