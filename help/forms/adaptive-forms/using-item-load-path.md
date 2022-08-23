---
title: Uso de la ruta de carga de elementos para rellenar la lista desplegable
description: Configure y rellene una lista desplegable para leer valores de un nodo crx
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
source-git-commit: 614db8b03a823b60846ab8ccfa8fbc29a41f7791
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 0%

---

# Propiedad de carga de elementos en AEM Forms

Configure y rellene la lista desplegable con la propiedad ruta de carga del elemento.
El campo Ruta de carga del elemento permite que un autor proporcione una dirección URL desde la que carga las opciones disponibles en una lista desplegable.
Para crear un nodo de este tipo en crx, siga los pasos que se mencionan a continuación

* Iniciar sesión en crx
* Cree un nodo llamado assets (puede asignar un nombre a este nodo según sus necesidades) en el tipo sling:folder , en el contenido.
* Guardar
* Haga clic en el nodo de recursos recién creado y defina sus propiedades como se muestra a continuación
* Deberá crear una propiedad de tipo String denominada assettypes (puede asignarle el nombre que necesite) con multivalor. Proporcione los valores que desee y guarde.

![item-load-path](assets/item-load-path-crx.png)

Para cargar estos valores en la lista desplegable, proporcione la siguiente ruta en la propiedad ruta de carga del elemento  **/content/assets/assettypes**

El paquete de muestra puede ser [descargado desde aquí](assets/item-load-path-package.zip)
