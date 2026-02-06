---
title: Solucionar problemas de canalización de CI/CD mediante el agente de desarrollo de AEM
description: Obtenga información sobre cómo solucionar problemas y corregir una canalización de CI/CD que ha fallado mediante el Agente de desarrollo de AEM.
version: Experience Manager as a Cloud Service
role: Developer, Admin
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20279
thumbnail: KT-20279.png
last-substantial-update: null
source-git-commit: 6fa0f88c231f7b68392a77a60491d4f741140a5a
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 1%

---


# Solucionar problemas de canalización de CI/CD mediante el agente de desarrollo de AEM

Obtenga información sobre cómo solucionar problemas y corregir una canalización de CI/CD que ha fallado mediante el Agente de desarrollo de AEM.

El Agente de desarrollo de AEM ayuda a equipos técnicos, incluidos desarrolladores, ingenieros de DevOps y administradores a **acelerar sus flujos de trabajo** al proporcionar _orientación y acciones con tecnología de IA_.

>[!TIP]
>
> Consulte también [Información general sobre agentes en AEM](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) para obtener una lista completa de los agentes disponibles en AEM as a Cloud Service, su funcionalidad y cómo puede obtener acceso a ellos.


## Información general

El Agente de desarrollo de AEM ofrece varias funciones, incluida la capacidad de enumerar, solucionar problemas y corregir canalizaciones de CI/CD fallidas. Puede invocar el agente de desarrollo de AEM a través del asistente de IA para abordar sus casos de uso específicos.

Este tutorial utiliza el [Proyecto de sitios WKND](https://github.com/adobe/aem-guides-wknd) para mostrar cómo solucionar problemas y corregir una canalización de CD/CI con errores mediante el Agente de desarrollo de AEM. Los mismos principios se aplican a cualquier proyecto de AEM.

Para simplificar, este tutorial presenta un error de prueba unitaria en el archivo `BylineImpl.java` para mostrar las capacidades de solución de problemas de canalización del Agente de desarrollo de AEM.

## Requisitos previos

Para seguir este tutorial, necesita lo siguiente:

- Asistente de IA y agentes en AEM habilitados. Consulte [Configurar IA en AEM](./setup.md) para obtener más información y tenga en cuenta que los parques de reproducción mencionados en ese artículo no tendrán las capacidades del Agente de desarrollo de AEM.
- Acceso a Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) con un rol de Desarrollador o Administrador de programas. Consulte [definiciones de funciones](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-manager/content/requirements/users-and-roles#role-definitions) para obtener más información.
- Un entorno de AEM as a Cloud Service
- Acceso a agentes en AEM a través del [programa Beta](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)
- El [proyecto de sitios WKND](https://github.com/adobe/aem-guides-wknd) se clonó en el equipo local

### Capacidades actuales de AEM Development Agent

Antes de sumergirse en el tutorial, vamos a revisar las capacidades actuales del Agente de desarrollo de AEM:

- Lista de canalizaciones de CI/CD y su estado
- Solucionar problemas y corregir **canalizaciones full-stack** con errores, incluidos los tipos _Calidad del código_ y _Implementación_.
- Se admiten los pasos _Build_ (compilación del código para producir un artefacto implementable) y _Code Quality_ (análisis de código estático mediante reglas SonarQube) de las canalizaciones **full-stack**.

Las capacidades del Agente de desarrollo de AEM se amplían y actualizan continuamente de forma regular. Para recibir comentarios y sugerencias, envíe un correo electrónico a [aem-devagent@adobe.com](mailto:aem-devagent@adobe.com).

## Configuración

Siga estos pasos de alto nivel para completar este tutorial:

1. Clone el [Proyecto WKND Sites](https://github.com/adobe/aem-guides-wknd) y envíelo a su repositorio Git de Cloud Manager
2. Creación y configuración de una canalización de calidad de código
3. Ejecute la canalización y observe la ejecución fallida
4. Utilice el Agente de desarrollo de AEM para solucionar problemas y corregir la canalización fallida

Veamos cada paso en detalle.

### Uso del proyecto de WKND Sites como proyecto de demostración

Este tutorial utiliza la rama `tutorial/dev-agent/unit-test-failure` del proyecto WKND Sites para mostrar cómo utilizar el agente de desarrollo de AEM. Los mismos principios se pueden aplicar a cualquier proyecto de AEM.

- Se ha introducido un error de prueba unitaria en el archivo `BylineImpl.java` de la siguiente manera. Si utiliza su propio proyecto de AEM, puede producir un error de prueba unitaria similar.

  ```java
  ...
  @Override
  public String getName() {
      if (name != null) {
          return "Author: " + name; // This line is intentionally incorrect to introduce a unit test failure.
      }
      return name;
  }
  ...
  ```

- Clone el [Proyecto WKND Sites](https://github.com/adobe/aem-guides-wknd) en su equipo local, vaya al directorio del proyecto y cambie a la rama `tutorial/dev-agent/unit-test-failure`.

  ```shell
  git clone https://github.com/adobe/aem-guides-wknd.git
  cd aem-guides-wknd
  git checkout tutorial/dev-agent/unit-test-failure
  ```

- Cree un nuevo repositorio Git de Cloud Manager para el proyecto WKND Sites y agréguelo como remoto al repositorio Git local:

   - Vaya a Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) y seleccione su programa.
   - Haga clic en **Repositorios** en la barra lateral izquierda.
   - Haga clic en **Agregar repositorio** en la esquina superior derecha.
   - Escriba un **Nombre de repositorio** (por ejemplo, &quot;wknd-site-tutorial&quot;) y haga clic en **Guardar**. Espere a que se cree el repositorio.

     ![Agregar repositorio](./assets/dev-agent/add-repository.png)

   - Haga clic en **Acceder a la info del repositorio** en la esquina superior derecha y copie la URL del repositorio.

     ![Acceder a la info del repositorio](./assets/dev-agent/access-repo-info.png)

   - Añada el repositorio Git de Cloud Manager recién creado como remoto al repositorio Git local:

     ```shell
     git remote add adobe https://git.cloudmanager.adobe.com/<your-adobe-organization>/wknd-site-tutorial/
     ```

- Inserte el repositorio Git local en el repositorio Git de Cloud Manager:

  ```shell
  git push adobe
  ```

  Cuando se le soliciten credenciales, proporcione **Nombre de usuario** y **Contraseña** del modal **Información del repositorio** de Cloud Manager.

### Creación y configuración de una canalización de calidad de código

Este tutorial utiliza una canalización de calidad del código (que no es de producción) para almacenar en déclencheur el error de la canalización para solucionar problemas. Consulte [Introducción a las canalizaciones de CI/CD](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#introduction) para obtener más información sobre las canalizaciones de calidad de código.

- En Cloud Manager, vaya a la sección **Canalizaciones** y seleccione **Agregar** > **Agregar canalización que no sea de producción**.
- En el cuadro de diálogo **Agregar canalización que no sea de producción**, configure lo siguiente:

   - Paso **Configuración**:
      - Mantenga los valores predeterminados como **Tipo de canalización** como `Code Quality Pipeline` y **Déclencheur de implementación** como `Manual`.
      - Para **Nombre de canalización que no es de producción**, escriba `Code Quality::Fullstack`

     ![Agregar configuración de canalización que no sea de producción](./assets/dev-agent/add-non-production-pipeline-configuration.png)

   - Paso **Código Source**:
      - Seleccionar **código de pila completa**
      - Para **Repositorio**, seleccione el repositorio Git de Cloud Manager recién creado
      - Para **Rama Git**, seleccione `tutorial/dev-agent/unit-test-failure`
      - Haga clic en **Guardar**.

     ![Agregar código Source de canalización que no sea de producción](./assets/dev-agent/add-non-production-pipeline-source-code.png)

- Ejecute la canalización Calidad del código recién creada haciendo clic en **Ejecutar** en el menú de tres puntos de la entrada de la canalización.

  ![Ejecutar canalización de calidad de código](./assets/dev-agent/run-code-quality-pipeline.png)


>[!IMPORTANT]
>
> La canalización de implementación no se trata en este tutorial. Sin embargo, puede seguir los mismos principios para solucionar problemas y corregir una canalización de implementación fallida.


### Observe la ejecución de la canalización fallida

Error en la canalización Calidad del código del paso **Preparación de artefactos**:

![Error de ejecución de canalización](./assets/dev-agent/failed-pipeline-execution.png)

Sin el Agente de desarrollo de AEM, este error de canalización requiere una solución de problemas manual. Un desarrollador tendría que comprobar los registros y revisar el código, un proceso tedioso y laborioso.

A continuación, verá cómo la inteligencia artificial aplicada a la actividad empresarial puede solucionar problemas y corregir la ejecución fallida de la canalización.

## Usar el agente de desarrollo de AEM para solucionar problemas y solucionar errores de canalización

Puede invocar el Agente de desarrollo de AEM mediante el Asistente de IA en AEM, describiendo el error de canalización en lenguaje natural.

- Haga clic en el icono **Ayudante de IA** en la esquina superior derecha.

- Escriba los detalles del error de canalización en lenguaje natural conocido como **Prompt**. Por ejemplo:

  ```text
  I have a failed pipeline execution on %PROGRAM-NAME% program, help me to troubleshoot and fix it.
  ```

  ![Invocar el agente de desarrollo de AEM](./assets/dev-agent/invoke-aem-development-agent.png)

  **Agente de desarrollo de AEM** se invoca para solucionar problemas y corregir la ejecución de la canalización con errores.

  >[!NOTE]
  >
  > Si el mensaje introducido no está claro, el asistente de inteligencia artificial pide aclaraciones y proporciona información para ayudarle a refinar el mensaje.

- Una vez que finalice el razonamiento, haga clic en el icono **Abrir en pantalla completa** para ver el proceso detallado de solución de problemas.

  ![Abrir en pantalla completa](./assets/dev-agent/open-in-full-screen.png)

  Los resultados contienen información valiosa, incluidos detalles del error, el archivo de origen, el número de línea y una sección **Cómo corregir** con pasos claros para resolver el problema.

- En este caso, el agente sugirió correctamente cambiar la implementación (`getName()` método) o actualizar la prueba unitaria (`getNameTest()` método) para solucionar el problema. Evitó las alucinaciones y utilizó un enfoque de &quot;ser humano en el bucle&quot; mientras proporcionaba cambios de código procesables para el desarrollador.

  ![Copiar cambios de código](./assets/dev-agent/copy-code-changes.png)

- Actualice el archivo `BylineImpl.java` con los cambios de código sugeridos, luego confirme e inserte los cambios en el repositorio Git de Cloud Manager.

  ```java
  ...
  @Override
  public String getName() {
      return name;
  }
  ...
  ```

- Vuelva a ejecutar la canalización y observe la ejecución correcta.

## Ejemplos adicionales

El proyecto de WKND Sites incluye ejemplos adicionales de código dañado y problemas de configuración, como dependencias que faltan y configuración incorrecta. Puede explorar estos ejemplos comprobando las [ramas que comienzan con `tutorial/dev-agent/`](https://github.com/adobe/aem-guides-wknd/branches/all?query=tutorial%2Fdev-agent&lastTab=overview). Para ver los cambios importantes, puede comparar la rama `tutorial/dev-agent/unit-test-failure` con la rama `main` haciendo clic en el botón **Comparar**. Luego busque la sección _archivo modificado_.

![Comparar ramas](./assets/dev-agent/compare-branches.png)

Vea también [Ejemplos de mensajes](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview#sample-prompts) para obtener más ideas sobre cómo usar el Agente de desarrollo de AEM.

## Resumen

En este tutorial, ha aprendido a utilizar el Agente de desarrollo de AEM para solucionar problemas y corregir una canalización de CI/CD que ha fallado mediante el Asistente de IA. También ha aprendido cómo la inteligencia artificial aplicada a la actividad empresarial acelera los flujos de trabajo técnicos al proporcionar perspectivas procesables y cambios de código.

Empiece a utilizar el Agente de desarrollo de AEM y otros agentes en AEM para acelerar los flujos de trabajo. Vea [Información general sobre los agentes en AEM](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) para obtener más información.

## Recursos adicionales

- [IA en Experience Manager](./overview.md)
- [Información general de agentes en AEM](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
- [Descripción general del agente de desarrollo](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview)
- [Información general de agentes en AEM](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
