---
title: Cree la estructura para el componente de países
description: Crear la estructura del componente en crx
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: 87e790c9-6ef6-4337-90b8-687ca576b21a
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 2%

---

# Cree la estructura para el componente de países

Inicie sesión en la instancia de AEM Forms y siga estos pasos para crear un nuevo componente basado en el componente desplegable predeterminado:

1. Vaya a &quot;/apps/&lt;yourproject>/components/adaptiveForm/dropdown&quot; en CRXDE Lite.
2. Copie el componente desplegable y péguelo en el mismo nivel de directorio.
3. Cambie el nombre del componente copiado a países.
4. Actualice la propiedad jcr:title del nodo cq:template a Países.
5. Guarde los cambios.

Ahora tiene un nuevo componente llamado Países, que es una réplica exacta del componente desplegable predeterminado. Esto sirve de base para una mayor personalización.

## Cree el archivo HTL

Para crear el archivo HTL para el componente Países:

1. Vaya a la carpeta de países en el repositorio crx
2. Cree un nuevo archivo llamado countries.html.
3. Abra el archivo /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html en el repositorio crx y copie su contenido.
4. Pegue el contenido copiado en countries.html.
5. Cambie el código para utilizar el nuevo modelo de sling como se muestra en la captura de pantalla
6. Guarde los cambios.

![modelo sling](assets/countriesdropdown.png)

Finalmente, sincronice el proyecto con estas actualizaciones para asegurarse de que los cambios en el repositorio de CRX se reflejen en el proyecto de AEM.


## Siguientes pasos

[Crear cuadro de diálogo cq](./dialog.md)
