---
user-guide-title: Tutoriales de Adobe Experience Manager as a Cloud Service
user-guide-description: Una recopilación de tutoriales de Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriales de AEM as a Cloud Service
sub-product: cloud-service
team: TM
source-git-commit: 4c9d836881ad7cccd31c55fa5eddc24dff1200cd
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 21%

---


# Tutoriales de Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Información general](./overview.md)
+ Introducción a AEM as a Cloud Service{#introduction}
   + [¿Qué es AEM como Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Evolución](./introduction/evolution.md)
   + [Arquitectura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
+ Tecnología subyacente {#underlying-technology}
   + [Arquitectura AEM](./underlying-technology/introduction-architecture.md)
   + [los paquetes](./underlying-technology/introduction-osgi.md)
   + [Repositorio de contenido Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Servicios de creación y publicación](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [Programas](./cloud-manager/programs.md)
   + [Entornos](./cloud-manager/environments.md)
   + [Canalización de producción de CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Canalización de no producción de CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Actividad](./cloud-manager/activity.md)
   + Operaciones de desarrollo{#devops}
      + [Implementación de código](./cloud-manager/devops/deploy-code.md)
      + [Combinar proyectos](./cloud-manager/devops/merge-projects.md)
      + [Configurar canalizaciones](./cloud-manager/devops/configure-pipelines.md)
      + [Integración continua](./cloud-manager/devops/continuous-integration.md)
      + [Analizar resultados de la prueba](./cloud-manager/devops/analyze-test-results.md)
      + [Configuraciones de Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [API de Cloud Manager](./cloud-manager/devops/cloud-manager-apis.md)
+ Configuración del entorno de desarrollo local {#local-development-environment-set-up}
   + [Información general](./local-development-environment/overview.md)
   + [Herramientas de desarrollo](./local-development-environment/development-tools.md)
   + [Tiempo de ejecución de AEM local](./local-development-environment/aem-runtime.md)
   + [Herramientas locales de Dispatcher](./local-development-environment/dispatcher-tools.md)
+ Desarrollo de{#developing}
   + Conceptos básicos de desarrollo{#basics}
      + [SDK AEM](./developing/basics/aem-sdk.md)
      + [Entorno de desarrollo local](./developing/basics/local-development-environment.md)
      + [Tipo de archivo del proyecto AEM](./developing/basics/aem-project-archetype.md)
      + [Estructura del proyecto AEM](./developing/basics/project-structure.md)
      + [Contenido mutable vs. inmutable](./developing/basics/mutable-immutable.md)
      + [Paquete de estructura del repositorio](./developing/basics/repository-structure-package.md)
      + [Publicación de contenido](./developing/basics/content-publishing.md)
      + [Configuraciones de OSGi](./developing/basics/osgi-configurations.md)
      + [Migración de configuración de Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Proyectos AEM{#aem-projects}
      + [AEM proyecto Maven](./developing/projects/maven-project-structure.md)
   + Servicios OSGi{#osgi-services}
      + [Conceptos básicos del servicio OSGi](./developing/osgi-services/basics.md)
      + [Ciclo de vida de los componentes OSGi](./developing/osgi-services/lifecycle.md)
      + [Conceptos básicos de configuraciones de OSGi](./developing/osgi-services/configurations.md)
      + [Configuraciones de OSGi que utilizan OCD](./developing/osgi-services/configurations-ocd.md)
   + [API de SDK de AEM JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ Depuración AEM{#debugging}
   + Depuración del SDK de AEM{#debugging-aem-sdk}
      + [Información general](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registros](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuración remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Consola web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Herramientas de Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Otras herramientas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depuración AEM como Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Información general](./debugging/cloud-service/overview.md)
      + [Registros](./debugging/cloud-service/logs.md)
      + [Compilación e implementación](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ Acceso a AEM{#accessing}
   + [Información general](./accessing/overview.md)
   + [Adobe de usuarios de IMS](./accessing/adobe-ims-users.md)
   + [Adobe de grupos de usuarios de IMS](./accessing/adobe-ims-user-groups.md)
   + [Perfiles de producto IMS de Adobe](./accessing/adobe-ims-product-profiles.md)
   + [AEM usuarios, grupos y permisos](./accessing/aem-users-groups-and-permissions.md)
   + [Configuración del acceso a AEM guía](./accessing/walk-through.md)
+ Migración {#migration}
   + [Herramienta de transferencia de contenido](./migration/content-transfer-tool.md)
   + [Importación masiva de recursos](./migration/bulk-import.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [Introducción](./migration/cloud-acceleration-manager/introduction.md)
      + [Analizador de preparación y prácticas recomendadas](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Fase de implementación](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Herramienta de transferencia de contenido](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [Herramientas de refactorización de código](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernizador del repositorio de código](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Conversor de índices](./migration/cloud-acceleration-manager/index-converter.md)
      + [Herramienta de Migración del flujo de trabajo de recursos](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navegación por Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Uso del Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}
   + Crear formulario adaptable{#create-first-af}
      + [Introducción](./forms/create-first-af/introduction.md)
      + [Crear tema](./forms/create-first-af/create-theme.md)
      + [Crear plantilla](./forms/create-first-af/create-template.md)
      + [Crear fragmento](./forms/create-first-af/create-fragments.md)
      + [Crear formulario](./forms/create-first-af/create-af.md)
      + [Configuración del panel raíz](./forms/create-first-af/configure-root-panel.md)
      + [Configuración del panel Personas](./forms/create-first-af/configure-people-panel.md)
      + [Configuración del panel de ingresos](./forms/create-first-af/configure-income-panel.md)
      + [Configuración del panel de recursos](./forms/create-first-af/configure-assets-panel.md)
      + [Configuración del panel de inicio](./forms/create-first-af/configure-start-panel.md)
      + [Agregar y configurar barra de herramientas](./forms/create-first-af/add-configure-toolbar.md)
   + API de Document Cloud y AEM Forms CS{#doc-cloud-sdk}
      + [Introducción](./forms/doc-cloud-sdk/introduction.md)
      + [Crear proyecto de Adobe I/O](./forms/doc-cloud-sdk/create-document-cloud-credentials.md)
      + [Crear la configuración OSGi](./forms/doc-cloud-sdk/create-doc-cloud-configuration.md)
      + [Definir interfaz](./forms/doc-cloud-sdk/create-interface.md)
      + [Implementar interfaz](./forms/doc-cloud-sdk/implement-interface.md)
      + [Crear parte de JSON](./forms/doc-cloud-sdk/get-content-analyzer.md)
      + [Paso de proceso personalizado](./forms/doc-cloud-sdk/custom-process-step.md)
   + Almacenamiento de Azure Portal{#forms-cs-azure-portal}
      + [Introducción](./forms/forms-cs-azure-portal/introduction.md)
      + [Crear modelo de datos de formulario](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Almacenar datos de formulario en Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Formulario de rellenado previo](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Envíos de consultas](./forms/forms-cs-azure-portal/query-submitted-data.md)


      + Crear flujo de trabajo de revisión{#create-aem-workflow}
         + [Crear modelo de flujo de trabajo](./forms/create-aem-workflow/create-workflow.md)
         + [flujo de trabajo de déclencheur](./forms/create-aem-workflow/configure-af.md)
      + Adobe Sign con AEM Forms{#forms-and-sign}
         + [Introducción](./forms/forms-and-sign/introduction.md)
         + [Aplicación de API de Adobe Sign](./forms/forms-and-sign/create-sign-api-application.md)
         + [Configuración en la nube de Adobe Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
         + [Crear formulario adaptable](./forms/forms-and-sign/create-adaptive-form.md)
         + [Configurar para rellenar y firmar](./forms/forms-and-sign/configure-form-fill-and-sign.md)
      + Integrar con Salesforce{#integrate-with-salesforce}
         + [Introducción](./forms/integrate-with-salesforce/introduction.md)
         + [Crear aplicación conectada](./forms/integrate-with-salesforce/create-connected-app.md)
         + [Crear archivo de intercambio](./forms/integrate-with-salesforce/describe-rest-api.md)
         + [Crear fuente de datos](./forms/integrate-with-salesforce/create-data-source.md)
         + [Crear modelo de datos de formulario](./forms/integrate-with-salesforce/create-form-data-model.md)
         + [Envío de formulario de prueba](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
         + [Evento de clic de prueba](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ Extensibilidad de asset compute{#asset-compute}
   + [Información general](./asset-compute/overview.md)
   + Configurar{#set-up}
      + [Aprovisionamiento de cuentas y servicios](./asset-compute/set-up/accounts-and-services.md)
      + [Entorno de desarrollo local](./asset-compute/set-up/development-environment.md)
      + [Luciérnagas del proyecto Adobe](./asset-compute/set-up/firefly.md)
   + Desarrollar{#develop}
      + [Creación de un proyecto de Asset compute](./asset-compute/develop/project.md)
      + [Configuración de variables de entorno](./asset-compute/develop/environment-variables.md)
      + [Configurar manifest.yml](./asset-compute/develop/manifest.md)
      + [Desarrollo de un trabajador](./asset-compute/develop/worker.md)
      + [Uso de la herramienta de desarrollo](./asset-compute/develop/development-tool.md)
   + Probar y depurar{#test-debug}
      + [Probar un trabajador](./asset-compute/test-debug/test.md)
      + [Depurar un trabajador](./asset-compute/test-debug/debug.md)
   + Implementar{#deploy}
      + [Implementar en Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrar con AEM](./asset-compute/deploy/processing-profiles.md)
   + Avanzado {#advanced}
      + [Trabajadores de metadatos](./asset-compute/advanced/metadata.md)
   + [Solución de problemas](./asset-compute/troubleshooting.md)
+ Tutorials de varios pasos{#multi-step-tutorials}
   + [Desarrollo de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html)
   + [SPA Editor (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [SPA Editor (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites y Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticación basada en tokens](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
