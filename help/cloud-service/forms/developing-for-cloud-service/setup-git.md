---
title: Instalación y configuración de Git
description: Inicialice el repositorio de Git local
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 48
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 1%

---

# Instalar Git


[Instalar Git](https://git-scm.com/downloads). Puede seleccionar la configuración predeterminada y completar el proceso de instalación.
Vaya al símbolo del sistema
Vaya a c:\cloudmanager\aem-banking-app
escriba en git —version. Debería ver la versión de GIT instalada en el sistema

## Inicializar el repositorio Git local

Asegúrese de que está en la carpeta c:\cloudmanager\aem-banking-app

```
git init
```

El comando anterior inicializará el proyecto como un repositorio local de Git

```
git add .
```

Esto agrega todos los archivos de proyecto al repositorio de Git listos para enviarse al repositorio de Git

```
git commit -m "initial commit"
```

Esto confirma los archivos en el repositorio de Git



## Registre el repositorio de Cloud Manager con nuestro repositorio local de Git

Acceder a su repositorio de Cloud Manager
![acceder a la información del representante](assets/cloud-manager-repo.png)
Obtener las credenciales del repositorio de Cloud Manager
![get-credentials](assets/cloud-manager-repo1.png)

Guarde el nombre de usuario en el archivo de configuración

```java
git config --global credential.username "gbedekar-adobe-com"
```

guardar la contraseña en el archivo de configuración

```java
git config --global user.password "XXXX"
```

(La contraseña es la contraseña del repositorio de Git de Cloud Manager)

Registre el repositorio de Git de Cloud Manager con el repositorio de Git local. El comando siguiente asocia **bankingapp** con el repositorio de Git remoto de Cloud Manager. Podrías haber usado cualquier nombre en lugar de **bankingapp**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Asegúrese de utilizar la URL del repositorio)

Comprobar si el repositorio remoto está registrado

```java
git remote -v
```

## Siguientes pasos

[AEM AEM Sincronizar con el proyecto de en IntelliJ](./intellij-and-aem-sync.md)
