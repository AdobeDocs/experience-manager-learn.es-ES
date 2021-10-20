---
title: Insertar AEM proyecto en el repositorio de Cloud Manager
description: Insertar el repositorio de Git local al repositorio de Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8851
source-git-commit: d42fd02b06429be1b847958f23f273cf842d3e1b
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# Insertar AEM proyecto en el representante de Git de Cloud Manager

En el paso anterior, sincronizamos nuestro proyecto AEM con el Forms adaptable y los temas creados en la instancia de AEM.
Ahora necesitamos agregar estos cambios a nuestro repositorio de Git local y luego insertar el repositorio de Git local en el repositorio de Git de Cloud Manager

abra el símbolo del sistema y vaya a c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .**
```

Esto agrega los nuevos archivos a la rama de etapa del repositorio de Git local

```
git commit -m "My First AF"
```

Esto confirma los archivos a la rama maestra de nuestro repositorio de Git local

```
git push -f bankingapp master:"My First AF"
```

En el comando anterior estamos empujando nuestra rama maestra desde nuestro repositorio de Git local a la rama My First AF del repositorio de cloud manager identificada por el nombre descriptivo de la aplicación de banca



