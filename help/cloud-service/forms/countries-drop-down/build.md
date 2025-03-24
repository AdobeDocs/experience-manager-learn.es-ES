---
title: Creación, implementación y prueba del componente de países
description: Generar, implementar y probar
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: ab9bd406-e25e-4e3c-9f67-ad440a8db57e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Creación, implementación y prueba del componente de países

Para generar todos los módulos e implementar el paquete `all` en una instancia local de AEM, ejecute el siguiente comando en el directorio raíz del proyecto:

```mvn clean install -PautoInstallSinglePackage```

## Prueba del componente

Para integrar el componente Países en la instancia de AEM Forms Cloud Ready y configurarlo, siga estos pasos:

* Extraiga el contenido del archivo zip [country](assets/countries.zip). Cada archivo debe contener los datos de un continente específico.
* Cargue los archivos json en content/dam/corecomponent.This, donde el código busca los archivos json. Si desea almacenar los archivos JSON en una ubicación diferente, deberá actualizar el código Java en la clase CountryDropDownImpl. Concretamente, actualice la ruta en el método init() donde se cargan los archivos JSON. Por ejemplo, si desea almacenar los archivos JSON en content/dam/mydata/, actualice la ruta de esta manera

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* Inicie sesión en la instancia de AEM Forms Cloud Ready
* Cree un formulario adaptable y suelte el componente Países en el formulario
* Configure el componente Países mediante el editor de diálogos y establezca las distintas propiedades, incluido el continente
  ![contenido](assets/select-continent.png)
* Previsualice el formulario y asegúrese de que la lista desplegable de países funciona según lo esperado
