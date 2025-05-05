---
title: Funciones personalizadas en AEM Forms
description: Crear y utilizar funciones personalizadas en formularios adaptables
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
duration: 286
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---

# Funciones personalizadas

AEM Forms 6.5 ha introducido la capacidad de definir las funciones de JavaScript que se pueden utilizar para definir reglas comerciales complejas mediante el editor de reglas.
AEM Forms proporciona varias de estas funciones personalizadas de forma predeterminada, pero tendrá que definir sus propias funciones personalizadas y utilizarlas en varios formularios.

Para definir la primera función personalizada, siga los siguientes pasos:
* [Iniciar sesión en crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Cree una nueva carpeta en las aplicaciones llamada experience-league (este nombre de carpeta puede ser un nombre de su elección)
* Guarde los cambios.
* En la carpeta de Experience League, cree un nuevo nodo de tipo cq:ClientLibraryFolder llamado clientlibs.
* Seleccione la carpeta clientlibs recién creada y agregue las propiedades allowProxy y categories como se muestra en la captura de pantalla y guarde los cambios.

![client-lib](assets/custom-functions.png)
* Cree una carpeta llamada **js** en la carpeta **clientlibs**
* Cree un archivo llamado **functions.js** en la carpeta **js**
* Cree un archivo llamado **js.txt** en la carpeta **clientlibs**. Guarde los cambios.
* La estructura de carpetas debe ser similar a la captura de pantalla siguiente.

![Editor de reglas](assets/folder-structure.png)

* Haga doble clic en functions.js para abrir el editor.
Copie el siguiente código en functions.js y guarde los cambios.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @returns {string[]} An array of county names
 */
function getCountyNamesList()
{
    return ["Santa Clara", "Alameda", "Buxor", "Contra Costa", "Merced"];

}
/**
* Covert UTC to Local Time
* @name convertUTC Convert UTC Time to Local Time
* @param {string} strUTCString in Stringformat
* @return {string}
*/
function convertUTC(strUTCString)
{
    var dt = new Date(strUTCString);
    console.log(dt.toLocaleString());
    return dt.toLocaleString();
}
```

[Consulte jsdoc](https://jsdoc.app/index.html)para obtener más información sobre cómo anotar funciones de javascript.
El código anterior tiene dos funciones:
**getCountyNamesList** - devuelve una matriz de cadena
**convertUTC**: convierte la marca de tiempo UTC a la zona horaria local.

Abra el archivo js.txt, pegue el siguiente código y guarde los cambios.

```javascript
#base=js
functions.js
```

La línea #base=js especifica en qué directorio se encuentran los archivos JavaScript.
Las líneas siguientes indican la ubicación del archivo JavaScript en relación con la ubicación base.

Si tienes problemas para crear las funciones personalizadas, [descarga e instala este paquete](assets/custom-functions.zip) en tu instancia de AEM.

## Uso de las funciones personalizadas

El siguiente vídeo le guía por los pasos necesarios para utilizar la función personalizada en el editor de reglas de un formulario adaptable
>[!VIDEO](https://video.tv.adobe.com/v/3445844?quality=12&learn=on&captions=spa)
