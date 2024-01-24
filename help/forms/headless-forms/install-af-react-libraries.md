---
title: Instale las bibliotecas de React del formulario adaptable requeridas
description: Añada las dependencias necesarias al proyecto de React
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
duration: 54
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 1%

---

# Instalación de las dependencias requeridas

Para empezar a utilizar formularios adaptables sin encabezado en su proyecto de react, instale las siguientes dependencias en su proyecto de react

* @aemforms/af-react-components
* @aemforms/af-react-renderer

Actualice el archivo package.json para incluir las siguientes dependencias. En el momento de escribir 0.22.41 era la versión actual

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>La lista desplegable y el diseño de tarjeta de este tutorial se han creado utilizando [Biblioteca de IU de material](https://mui.com/). Deberá descargar los paquetes de interfaz de usuario de material adecuados para que el código funcione en el sistema.

## Configurar proxy

El Intercambio de Recursos de Origen Cruzado (CORS) es un mecanismo de seguridad que restringe a los navegadores web de realizar solicitudes a un dominio diferente al dominio en el que está alojada la aplicación. Pueden producirse errores de CORS al intentar recuperar datos de una API alojada en un dominio diferente. Al configurar un proxy, puede evitar las restricciones CORS y realizar solicitudes a la API desde la aplicación React. He utilizado el siguiente código en un archivo llamado setUpProxy.js en la carpeta src. **Asegúrese de cambiar el destino para que apunte a la instancia de publicación.**

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

También deberá instalar y agregar el **http-proxy-middleware** a su proyecto.

## Pasos siguientes

[Buscar el formulario para incrustar](./fetch-the-form.md)
