---
title: Creando componente de dirección
description: Creación de un nuevo componente principal de dirección en AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: 280c9a30-e017-4bc0-9027-096aac82c22c
source-git-commit: ed64dd303a384d48f76c9b8e8e925f5d3b8f3247
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 3%

---

# Crear componente de dirección

Inicie sesión en CRXDE de la instancia local de AEM Forms lista para la nube.

Realice una copia del nodo ``/apps/bankingapplication/components/adaptiveForm/button`` y cambie su nombre a bloque de direcciones. Seleccione el nodo addressesblock y establezca sus propiedades como se muestra a continuación.

>[!NOTE]
>
> ``bankingapplication`` es el appId proporcionado al crear el proyecto Maven. Este appId podría ser diferente en su entorno. Puede hacer una copia de cualquier componente. Acabo de hacer una copia del componente del botón


![bloque de direcciones](assets/address-properties.png)

## propiedades del nodo cq-template

Seleccione el nodo ``cq-template`` bajo el nodo ``addressblock`` y establezca sus propiedades como se muestra a continuación. Observe que fieldType está configurado en el panel
![cq-template](assets/cq-template.png)

## Agregar nodos bajo cq-template

Agregue los siguientes nodos de tipo ``nt:unstructured`` en ``cq-template``

* street address
* ciudad
* zip
* estado

Estos nodos representan los campos del componente de bloque de direcciones. Los campos de dirección de calle, ciudad y código postal serán un campo de entrada de texto y el campo de estado será un campo desplegable.

## Establecer las propiedades del nodo streetaddress

>[!NOTE]
>
> La **_aplicación bancaria_** de la ruta hace referencia al appId del proyecto Maven. Esto puede ser diferente en su entorno

Seleccione el nodo ``streetaddress`` y establezca sus propiedades como se muestra a continuación.
![dirección-calle](assets/streetaddress.png)

## Establecer las propiedades del nodo de ciudad

Seleccione el nodo ``city`` y establezca sus propiedades como se muestra a continuación.
![ciudad](assets/city.png)

## Establecer las propiedades del nodo zip

Seleccione el nodo ``zip`` y establezca sus propiedades como se muestra a continuación.
![zip](assets/zip.png)

## Establecer las propiedades del nodo de estado

Seleccione el nodo ``state`` y establezca sus propiedades como se muestra a continuación. Observe el fieldType del estado: está configurado para ser un menú desplegable
![estado](assets/state.png)

## Definir opciones para el campo de estado

Seleccione el nodo ``state`` y agregue las siguientes propiedades.

| Nombre | Tipo | Valor  |
|----------|----------|---------------------|
| enum | Cadena [] | CA, NY |
| enumNames | Cadena [] | California, Nueva York |


El componente final de bloque de direcciones tendrá este aspecto

![dirección-final](assets/crx-address-block.png)

## Siguientes pasos

[Implemente el proyecto](./deploy-your-project.md)
