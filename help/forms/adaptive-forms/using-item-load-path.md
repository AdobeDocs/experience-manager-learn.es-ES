---
title: Uso de la ruta de carga de los elementos para rellenar la lista desplegable
description: Configure y rellene una lista desplegable para leer valores de un nodo crx
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Propiedad de carga de elementos en AEM Forms

Configure y rellene la lista desplegable con la propiedad de ruta de carga del elemento.
El campo Ruta de carga de elemento permite a un autor proporcionar una URL desde la que cargar las opciones disponibles en una lista desplegable.
Para crear un nodo de este tipo en crx, siga los pasos mencionados a continuación:
* Iniciar sesión en crx
* Cree un nodo llamado recursos (puede asignar un nombre a este nodo según sus necesidades) escriba sling:folder en contenido.
* Guardar
* Haga clic en el nodo de recursos recién creado y establezca sus propiedades como se muestra a continuación
* Deberá crear una propiedad de tipo cadena denominada assettypes (puede asignarle un nombre según sus necesidades). Asegúrese de que la propiedad sea de varios valores. Proporcione los valores que desee y guárdelos.
   ![item-load-path](assets/item-load-path-crx.png)

Para cargar estos valores en la lista desplegable, proporcione la siguiente ruta en la propiedad de ruta de carga del elemento  **/content/assets/assettypes**

El paquete de muestra puede ser [descargado desde aquí](assets/item-load-path-package.zip)
