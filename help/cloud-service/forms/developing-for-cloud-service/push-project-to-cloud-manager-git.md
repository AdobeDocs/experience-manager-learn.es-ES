---
title: Insertar proyecto de AEM en el repositorio de Cloud Manager
description: Insertar el repositorio local de Git en el repositorio de Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# Insertar proyecto de AEM en el repositorio de Git de Cloud Manager

En el paso anterior sincronizamos nuestro proyecto de AEM con el Forms adaptable y los temas creados en la instancia de AEM.
Ahora necesitamos agregar estos cambios a nuestro repositorio local de Git y luego insertar el repositorio local de Git en el repositorio de Git de Cloud Manager.
Abra el símbolo del sistema y vaya a c:\cloudmanager\aem-banking-app
Ejecute los siguientes comandos

```
git add .
```

Esto agrega los nuevos archivos a la rama de ensayo del repositorio de Git local

```
git commit -m "My First AF"
```

Esto confirma los archivos en la rama maestra del repositorio de Git local

```
git push -f bankingapp master:"MyFirstAF"
```

En el comando anterior estamos insertando nuestra rama maestra desde nuestro repositorio local de Git en la rama MyFirstAF del repositorio de Cloud Manager identificado por el nombre descriptivo de la aplicación bancaria.

## Siguientes pasos

[Implementar el proyecto en el entorno de desarrollo](./deploy-to-dev-environment.md)
