---
title: ¿Qué es Dispatcher?
description: Comprenda qué es realmente un Dispatcher.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 96c8dd09-e0a7-4abc-b04b-a805aaa67502
duration: 73
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 2%

---

# ¿Qué es Dispatcher?

[Tabla de contenidos](./overview.md)

Empezando por la descripción básica de lo que implica un Dispatcher de AEM.

## Servidor web Apache

Comience con una instalación básica del servidor web Apache en un servidor Linux.

Explicación básica de lo que hace un servidor Apache:

- Sigue reglas sencillas para proporcionar archivos a través de los protocolos HTTP(s) desde su directorio de documentos estático (`DocumentRoot`)
- Los archivos almacenados en una ubicación predeterminada (`/var/www/html`) coinciden en las solicitudes y se representan en el explorador del cliente solicitante




## Archivo de módulo específico de AEM (`mod_dispatcher.so`)

A continuación, agregue un complemento al servidor web Apache llamado módulo Dispatcher

Explicación básica de qué hace el módulo Dispatcher de Adobe AEM:

- Aumenta el controlador de archivos predeterminado
- Filtra las solicitudes incorrectas / Protege el estómago o los extremos de AEM
- Equilibrios de carga si hay más de un procesador
- Permite un directorio de caché activo / Admite el vaciado de archivos estancados
- Es la puerta principal de todas las instalaciones de AMS y ofrece sitios web y recursos al navegador del cliente
- Almacena en caché las solicitudes para reabastecerlas a una velocidad mucho más rápida de lo que un servidor de AEM podría lograr por sí solo
- Mucho más...

## Flujo de trabajo de tráfico web

Comprender qué partes se instalan juntas para crear un servidor de Dispatcher básico nos lleva a comprender el flujo de trabajo básico de tráfico web para una configuración de Adobe Manager Services.
Esto debería ayudarle a comprender qué papel desempeña en la cadena de sistemas que ofrecen contenido a los visitantes de su contenido de AEM.

<b>Sirviendo contenido ya almacenado en caché</b>

```
End User's Browser request 
    → Cloud Provider Load Balancer 
        → "The Dispatcher" 
            → Checks for cached request locally if found 
                → return request 
                    → End User
```

<b>Proporcionando contenido nuevo desde AEM</b>

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
