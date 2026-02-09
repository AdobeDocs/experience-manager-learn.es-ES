---
title: Búsqueda y eliminación de API obsoletas en AEM as a Cloud Service
description: Obtenga información sobre cómo encontrar y eliminar API obsoletas en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
source-git-commit: 6c5b911d1d59573338dd1a30eb95289bc1339f19
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 3%

---


# Búsqueda y eliminación de API obsoletas en AEM as a Cloud Service

Obtenga información sobre cómo encontrar y eliminar API obsoletas en AEM as a Cloud Service.

## Información general

El **Centro de actividades** de AEM as a Cloud Service le notifica sobre _API obsoletas_ en su proyecto. Para obtener las últimas funciones, actualizaciones de seguridad e implementaciones sin problemas del código en AEM as a Cloud Service mediante canalizaciones de Cloud Manager, elimine las API obsoletas del proyecto.

En este tutorial, aprenderá a buscar y eliminar las API obsoletas en su entorno de AEM as a Cloud Service mediante el [complemento Maven de AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

## Cómo encontrar las API obsoletas

Siga estos pasos para encontrar las API obsoletas en su proyecto de AEM as a Cloud Service.

1. **Usar el complemento Maven más reciente de AEM Analyzer**

   En su proyecto de AEM, use la versión más reciente del [complemento Maven de AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

   - En el(la) principal `pom.xml`, la versión del complemento generalmente se declara. Compare su versión con la última [versión lanzada](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin).

     ```xml
     ...
     <aemanalyser.version>1.6.14</aemanalyser.version> <!-- Latest released version as of 09-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - El complemento se compara con el último AEM SDK disponible. Utilice la última versión de AEM SDK en el archivo `pom.xml` de su proyecto. Ayuda a que aparezcan las API obsoletas como advertencias del IDE.

     ```xml
     ...
     <aem.sdk.api>2026.2.24288.20260204T121510Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 09-Feb-2026 -->
     ...
     ```

   - Asegúrese de que el módulo `all` ejecute el complemento en la fase `verify`.

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **Ejecutar una compilación y comprobar si hay advertencias**

   Cuando se ejecuta `mvn clean install`, el analizador informa de las API obsoletas como **[ADVERTENCIA]** mensajes en la salida. Por ejemplo:

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   Es fácil pasar por alto estos mensajes cuando se centra en el éxito o el fracaso de la compilación.

3. **Obtener una lista clara de las API obsoletas**

   El paso anterior también proporciona la misma información. Sin embargo, ejecute la fase `verify` en el módulo `all` para ver todos los mensajes de **[ADVERTENCIA]** en un solo lugar. Por ejemplo:

   ```shell
   $ mvn clean verify -pl all
   ```

   Los mensajes **[ADVERTENCIA]** de la generación enumeran las API obsoletas del proyecto.

## Eliminación de las API obsoletas

El analizador de AEM informa de **qué** está obsoleto y proporciona la **recomendación** sobre cómo solucionarlo. Sin embargo, utilice la tabla siguiente para elegir la acción correcta y siga la documentación vinculada cuando necesite más detalles.

### Estrategia de corrección de API obsoleta

| Tipo de advertencia del analizador | Lo que indica | Acción recomendada | Referencia |
| --------------------- | ----------------- | ------------------ | --------- |
| API de AEM obsoleta | La API se va a eliminar de AEM as a Cloud Service | Reemplazar el uso por la API pública admitida | [Guía de eliminación de API](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Clase o paquete de AEM obsoleto | Ya no se admite el paquete o la clase | Código de refactorización para utilizar la alternativa recomendada | [API obsoletas](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| Biblioteca de terceros obsoleta | La biblioteca no será compatible con futuros SDK | Actualizar la dependencia y refactorizar el uso | [Directrices generales](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Patrones obsoletos de Sling/OSGi | Se han detectado anotaciones o API heredadas | Migración a las API modernas de Sling y OSGi | [Eliminación de patrones de Sling/OSGi](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Eliminación planificada (fecha futura) | La API sigue funcionando, pero la eliminación se aplica más adelante | Programar limpieza antes de aplicar la canalización | [Notas de la versión](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/release-notes/home) |

### Guía práctica

- Trate las advertencias del analizador como **errores futuros de canalización**, no mensajes opcionales.
- Corrija las API obsoletas localmente mediante **la última versión de AEM SDK**.
- Mantenga la salida del analizador limpia para evitar problemas durante futuras actualizaciones de AEM.

La corrección de las API obsoletas hace que el proyecto **sea seguro para la actualización y esté listo para la implementación**.

## Recursos adicionales

- [Complemento Maven de AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [Funciones y API obsoletas y eliminadas](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)

