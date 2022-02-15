---
title: Funciones personalizadas en AEM Forms
description: Crear y utilizar funciones personalizadas en el formulario adaptable
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
kt: 9685
source-git-commit: 15b57ec6792bc47d0041946014863b13867adf22
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# Funciones personalizadas

AEM Forms 6.5 ha introducido la capacidad de definir las funciones de JavaScript que se pueden utilizar para definir reglas comerciales complejas mediante el editor de reglas.
AEM Forms proporciona varias de estas funciones personalizadas listas para usar, pero tendrá que definir sus propias funciones personalizadas y utilizarlas en varios formularios.

Para definir su primera función personalizada, siga los siguientes pasos:
* [Iniciar sesión en crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Cree una nueva carpeta en aplicaciones denominada experience-league (este nombre de carpeta puede ser el nombre de su elección)
* Guarde los cambios.
* En la carpeta de la liga de experiencia, cree un nuevo nodo de tipo cq:ClientLibraryFolder llamado clientlibs.
* Seleccione la carpeta clientlibs recién creada y añada las propiedades allowProxy y categories como se muestra en la captura de pantalla y guarde los cambios.

![client-lib](assets/custom-functions.png)
* Cree una carpeta llamada **js** en el **clientlibs** carpeta
* Cree un archivo llamado **functions.js** en el **js** carpeta
* Cree un archivo llamado **js.txt** en el **clientlibs** carpeta. Guarde los cambios.
* La estructura de la carpeta debería ser similar a la captura de pantalla siguiente.

![Editor de reglas](assets/folder-structure.png)

* Haga doble clic en functions.js para abrir el editor.
Copie el siguiente código en functions.js y guarde los cambios.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @return {OPTIONS} drop down options 
 */
function getCountyNamesList()
{
    var countyNames= [];
	countyNames[0] = "Santa Clara";
	countyNames[1] = "Alameda";
	countyNames[2] = "Buxor";
    countyNames[3] = "Contra Costa";
    countyNames[4] = "Merced";

	return countyNames;

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

Por favor [consulte jsdoc ](https://jsdoc.app/index.html)para obtener más información sobre la anotación de funciones de javascript.
El código anterior tiene dos funciones:
**getCountyNamesList** - devuelve una matriz de cadenas
**convertirUTC** - Convierte la marca de hora UTC en una zona horaria local

Abra js.txt y pegue el siguiente código y guarde los cambios.

```javascript
#base=js
functions.js
```

La línea #base=js especifica en qué directorio se encuentran los archivos JavaScript.
Las líneas siguientes indican la ubicación del archivo JavaScript en relación con la ubicación base.

Si tiene problemas para crear las funciones personalizadas, asegúrese de [descargar e instalar este paquete](assets/custom-functions.zip) en su instancia de AEM.

## Uso de las funciones personalizadas

El siguiente vídeo le explica los pasos necesarios para utilizar la función personalizada en el editor de reglas de un formulario adaptable
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=9&learn=on)
