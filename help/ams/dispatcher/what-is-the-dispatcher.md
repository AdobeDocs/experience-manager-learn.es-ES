---
title: ¿Qué es Dispatcher?
description: Comprenda qué es realmente un Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# ¿Qué es Dispatcher?

[Tabla de contenidos](./overview.md)

AEM Empezando por la descripción básica de lo que implica un Dispatcher de la.

## Servidor web Apache

Comience con una instalación básica del servidor web Apache en un servidor Linux.

Explicación básica de lo que hace un servidor Apache:

- Sigue reglas sencillas para proporcionar archivos a través de los protocolos HTTP(s) desde su directorio de documentos estático (`DocumentRoot`)
- Los archivos almacenados en una ubicación predeterminada (`/var/www/html`) coinciden en las solicitudes y se representan en el explorador del cliente solicitante




## AEM archivo de módulo específico de la (`mod_dispatcher.so`)

A continuación, agregue un complemento al servidor web Apache llamado módulo Dispatcher

Explicación básica de qué hace el Adobe AEM del módulo de Dispatcher:

- Aumenta el controlador de archivos predeterminado
- AEM Filtra las solicitudes incorrectas / Protege los puntos de conexión/vientre blando de la pantalla de protección de la
- Equilibrios de carga si hay más de un procesador
- Permite un directorio de caché activo / Admite el vaciado de archivos estancados
- Es la puerta principal de todas las instalaciones de AMS y ofrece sitios web y recursos al navegador del cliente
- AEM Almacena en caché las solicitudes de reutilización a una velocidad mucho más rápida de lo que un servidor de correo electrónico podría realizar por sí solo en la caché de un servidor de
- Mucho más...

## Flujo de trabajo de tráfico web

Comprender qué partes se instalan juntas para crear un servidor de Dispatcher básico nos lleva a comprender el flujo de trabajo básico de tráfico web para una configuración de Adobe Manager Services.
AEM Esto le ayudará a comprender qué papel desempeña en la cadena de sistemas que ofrecen contenido a los visitantes de su contenido de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la red de la.

<b>Sirviendo contenido ya almacenado en caché</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

AEM <b>Proporcionando contenido nuevo desde el sitio de trabajo</b> de la página de servicio

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

[Siguiente -> Diseño básico del archivo](./basic-file-layout.md)
