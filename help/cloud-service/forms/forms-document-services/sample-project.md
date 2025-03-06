---
title: Proyecto de ejemplo
description: Proyecto de ejemplo que se puede importar y ejecutar en su entorno
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---


# Probar en el entorno local

* Importar el proyecto

   * Descargar y extraer el [proyecto de ejemplo](./assets/formsdocumentservices.zip)
   * Abra su **Entorno de desarrollo de Java** preferido (IntelliJ IDEA, Eclipse o código VS) e importe el proyecto como proyecto Maven
* Configurar credenciales

   * Busque el archivo `resources/credentials/server_credentials.json`
   * Ábralo y **actualice las credenciales** específicas de su entorno.
   * Asegúrese de que incluye valores válidos para:
     `clientId`, `clientSecret`,`adobeIMSV3TokenEndpointURL` y
     `scopes`

* Ejecutar la clase Main

   * Vaya a `src/main/java/Main.java` y ejecute el método principal

* Verificar ejecución
   * Compruebe la salida en la ventana del terminal

