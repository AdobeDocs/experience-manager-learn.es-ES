---
title: Instalación y configuración de Git
description: Inicializar el repositorio de Git local
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
source-git-commit: 8d83d01fca3bfc9e6f674f7d73298b42f98a5d46
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Instalar Git


[Instalar Git](https://git-scm.com/downloads). Puede seleccionar la configuración predeterminada y completar el proceso de instalación.
Vaya al símbolo del sistema Vaya a c:\cloudmanager\aem-banking-app type in git —version. Debería ver la versión de GIT instalada en su sistema

## Inicializar repositorio Git local

Asegúrese de que está en c:\cloudmanager\aem-banking-app folder

```
git init
```

El comando anterior inicializará el proyecto como un repositorio local de Git

```
git add .
```

Esto agrega todos los archivos de proyecto al repositorio de Git listos para ser comprometidos con el repositorio de Git

```
git commit -m "initial commit"
```

Esto confirma los archivos en el repositorio de Git



## Registro del repositorio de Cloud Manager con nuestro repositorio local de Git

Acceda a su repositorio de cloud manager
![acceder a la información de la rep](assets/cloud-manager-repo.png)
Obtenga las credenciales del repositorio de cloud manager
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

Registre el repositorio de Git de Cloud Manager con su repositorio de Git local. El siguiente comando se asocia **aplicación de banca** con el repositorio de Git del administrador de nube remoto. Podría haber usado cualquier nombre en lugar de **aplicación de banca**


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Asegúrese de utilizar la URL de su repositorio)

Compruebe si el repositorio remoto está registrado

```java
git remote -v
```