---
title: Instalación de las bibliotecas de reacción de formularios adaptables necesarias
description: Añadir las dependencias necesarias al proyecto de respuesta
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 1%

---


# Instalación de las dependencias requeridas

Para empezar a utilizar formularios adaptables sin encabezado en el proyecto de Reacción, instale las siguientes dependencias en el proyecto de Reacción

* @aemforms/af-react-components
* @aemforms/af-react-renderer

Actualice package.json para incluir las siguientes dependencias. En el momento de escribir este artículo, 0.22.41 era la versión actual

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

## Configuración del proxy

El uso compartido de recursos de origen cruzado (CORS) es un mecanismo de seguridad que impide que los navegadores web realicen solicitudes en un dominio diferente al dominio en el que está alojada la aplicación. Los errores de CORS se pueden producir al intentar recuperar datos de una API alojada en un dominio diferente. Al configurar un proxy, puede evitar las restricciones CORS y realizar solicitudes a la API desde la aplicación React. He utilizado el siguiente código en un archivo llamado setUpProxy.js en la carpeta src. **Asegúrese de cambiar el destino para que apunte a la instancia de publicación.**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

También deberá instalar y agregar la variable **http-proxy-middleware** al proyecto.

## Pasos siguientes

[Buscar el formulario que se va a incrustar](./fetch-the-form.md)