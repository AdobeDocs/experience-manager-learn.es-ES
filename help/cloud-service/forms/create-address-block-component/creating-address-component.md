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
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# Crear componente de dirección

Inicie sesión en CRXDE de la instancia local de AEM Forms lista para la nube.

Haga una copia del ``/apps/bankingapplication/components/adaptiveForm/button`` y renómbrelo a bloqueDeDirecciones. Seleccione el nodo addressesblock y establezca sus propiedades como se muestra a continuación.

>[!NOTE]
>
> ``bankingapplication`` es el appId proporcionado al crear el proyecto Maven. Este appId podría ser diferente en su entorno. Puede hacer una copia de cualquier componente. Acabo de hacer una copia del componente del botón


![bloque de direcciones](assets/address-properties.png)

## propiedades del nodo cq-template

Seleccione el ``cq-template`` nodo bajo el ``addressblock`` y establezca sus propiedades como se muestra a continuación. Observe que fieldType está configurado en el panel
![cq-template](assets/cq-template.png)

## Agregar nodos bajo cq-template

Agregue los siguientes nodos de tipo ``nt:unstructured`` bajo ``cq-template``

* street address
* ciudad
* zip
* estado

Estos nodos representan los campos del componente de bloque de direcciones. Los campos de dirección de calle, ciudad y código postal serán un campo de entrada de texto y el campo de estado será un campo desplegable.

## Establecer las propiedades del nodo streetaddress

>[!NOTE]
>
> El **_aplicación bancaria_** en la ruta hace referencia al appId del proyecto de maven. Esto puede ser diferente en su entorno

Seleccione el ``streetaddress`` y establezca sus propiedades como se muestra a continuación.
![street-address](assets/streetaddress.png)

## Establecer las propiedades del nodo de ciudad

Seleccione el ``city`` y establezca sus propiedades como se muestra a continuación.
![ciudad](assets/city.png)

## Establecer las propiedades del nodo zip

Seleccione el ``zip`` y establezca sus propiedades como se muestra a continuación.
![zip](assets/zip.png)

## Establecer las propiedades del nodo de estado

Seleccione el ``state`` y establezca sus propiedades como se muestra a continuación. Observe el fieldType del estado: está configurado para ser un menú desplegable
![state](assets/state.png)

El componente final de bloque de direcciones tendrá este aspecto

![dirección-final](assets/crx-address-block.png)

## Siguientes pasos

[Implemente el proyecto](./deploy-your-project.md)




