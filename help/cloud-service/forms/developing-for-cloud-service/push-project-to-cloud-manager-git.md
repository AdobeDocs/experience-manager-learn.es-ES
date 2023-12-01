---
title: AEM Insertar proyecto en el repositorio de Cloud Manager
description: Insertar el repositorio local de Git en el repositorio de Cloud Manager
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8851
exl-id: e61cea37-b931-49c6-9e5d-899628535480
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 1%

---

# AEM Insertar proyecto de destino en el repositorio de Git de Cloud Manager

AEM En el paso anterior sincronizamos nuestro Proyecto de con el Forms AEM adaptable y las temáticas creadas en la instancia de la.
Ahora necesitamos agregar estos cambios a nuestro repositorio local de Git y luego insertar el repositorio local de Git en el repositorio de Git de Cloud Manager.
Abra el símbolo del sistema y vaya a c:\cloudmanager\aem-banking-app Ejecute los siguientes comandos

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

## Pasos siguientes

[Implementar el proyecto en el entorno de desarrollo](./deploy-to-dev-environment.md)
