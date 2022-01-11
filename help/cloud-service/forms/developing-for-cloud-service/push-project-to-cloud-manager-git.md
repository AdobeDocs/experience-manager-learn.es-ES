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
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# Insertar AEM proyecto en el repositorio de Git de Cloud Manager

En el paso anterior, sincronizamos nuestro proyecto AEM con el Forms adaptable y los temas creados en la instancia de AEM.
Ahora necesitamos agregar estos cambios a nuestro repositorio de Git local y luego insertar el repositorio de Git local en el repositorio de Git de Cloud Manager.
Abra el símbolo del sistema y vaya a c:\cloudmanager\aem-banking-app Execute the following commands

```
git add .
```

Esto agrega los nuevos archivos a la rama de etapa del repositorio de Git local

```
git commit -m "My First AF"
```

Esto confirma los archivos a la rama maestra de nuestro repositorio de Git local

```
git push -f bankingapp master:"MyFirstAF"
```

En el comando anterior estamos empujando nuestra rama maestra desde nuestro repositorio de Git local a la rama MyFirstAF del repositorio de cloud manager identificado por el nombre descriptivo de la aplicación de banca.
