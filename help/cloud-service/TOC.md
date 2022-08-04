---
user-guide-title: Tutoriales de Adobe Experience Manager as a Cloud Service
user-guide-description: Una recopilación de tutoriales de Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriales de AEM as a Cloud Service
sub-product: cloud-service
team: TM
source-git-commit: 3fb0fb5b8f43dc925da2ffa05808f24bf6d5ada3
workflow-type: tm+mt
source-wordcount: '818'
ht-degree: 22%

---


# Tutoriales de Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Información general](./overview.md)
+ Introducción a AEM as a Cloud Service{#introduction}
   + [¿Qué es AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Evolución](./introduction/evolution.md)
   + [Arquitectura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Estrategia y liderazgo mental{#strategy}
      + [Experience Manager - Modelos y arquetipos de administración y dotación de personal](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Cómo impulsar la velocidad de contenido con Adobe Experience Manager](./introduction/drive-content-velocity-for-sites.md)
      + [Acelerar la velocidad de contenido con sistemas de estilos AEM](./introduction/accelerate-content-velocity-aem.md)
+ [Integraciones de Experience Cloud](./experience-cloud/integrations.md)
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
      + [Limpieza de un proyecto AEM Maven](./developing/projects/remove-samples.md)
   + Servicios OSGi{#osgi-services}
      + [Conceptos básicos del servicio OSGi](./developing/osgi-services/basics.md)
      + [Ciclo de vida de los componentes OSGi](./developing/osgi-services/lifecycle.md)
      + [Conceptos básicos de configuraciones de OSGi](./developing/osgi-services/configurations.md)
      + [Configuraciones de OSGi que utilizan OCD](./developing/osgi-services/configurations-ocd.md)
   + Avanzado {#advanced}
      + [Usuarios de servicio](./developing/advanced/service-users.md)
      + [Almacenamiento en caché de variables de página](./developing/advanced/variant-caching.md)
   + [API de SDK de AEM JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ AEM de depuración{#debugging}
   + Depuración del SDK de AEM{#debugging-aem-sdk}
      + [Información general](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registros](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuración remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Consola web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Herramientas de Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Otras herramientas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depuración AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Información general](./debugging/cloud-service/overview.md)
      + [Registros](./debugging/cloud-service/logs.md)
      + [Compilación e implementación](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [Explorador del repositorio](./debugging/cloud-service/repository-browser.md)
      + Riesgos{#risks}
         + [Advertencias transitorias](./debugging/cloud-service/risks/traversals.md)
+ Acceso a AEM{#accessing}
   + [Información general](./accessing/overview.md)
   + [Usuarios de IMS de Adobe](./accessing/adobe-ims-users.md)
   + [Grupos de usuarios de IMS de Adobe](./accessing/adobe-ims-user-groups.md)
   + [Perfiles de producto de IMS de Adobe](./accessing/adobe-ims-product-profiles.md)
   + [AEM usuarios, grupos y permisos](./accessing/aem-users-groups-and-permissions.md)
   + [Configuración del acceso a AEM guía](./accessing/walk-through.md)
+ Autenticación{#authentication}
   + [Información general](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ Redes avanzadas{#networking}
   + [Información general](./networking/advanced-networking.md)
   + [Salida de puerto flexible](./networking/flexible-port-egress.md)
   + [Dirección IP de salida dedicada](./networking/dedicated-egress-ip-address.md)
   + [Red privada virtual](./networking/vpn.md)
   + Ejemplos de código{#examples}
      + [HTTP/HTTPS en puertos no estándar para salida de puerto flexible](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [HTTP/HTTPS para dirección IP de salida dedicada/VPN](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [Conexiones SQL usando DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Conexiones SQL usando API SQL de Java](./networking/examples/sql-java-apis.md)
      + [Servicio de correo electrónico](./networking/examples/email-service.md)
+ Migración {#migration}
   + [Herramienta de transferencia de contenido](./migration/content-transfer-tool.md)
   + [Importación masiva de recursos](./migration/bulk-import.md)

   + El paso de AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Introducción](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Incorporación](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA y CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM Herramientas de modernización](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernización del repositorio](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microservicios de asset compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Buscar e indexar](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migración de contenido {#content-migration}
         + [Servicio de importación masiva](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Herramienta de transferencia de contenido](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
      + [Solución de problemas](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Forms as a Cloud Service {#aem-forms}
         + [Introducción](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [Inscripción digital](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [Comunicaciones](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [Introducción](./migration/cloud-acceleration-manager/introduction.md)
      + [Analizador de preparación y prácticas recomendadas](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Fase de implementación](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Herramienta de transferencia de contenido](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [Herramientas de refactorización de código](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernizador del repositorio de código](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Conversor de índices](./migration/cloud-acceleration-manager/index-converter.md)
      + [Herramienta de migración del flujo de trabajo de recursos](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navegación por Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Uso del Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}

   + Desarrollo para Forms as a Cloud Service{#developing-for-cloud-service}
      + [Introducción](./forms/developing-for-cloud-service/getting-started.md)
      + [Instalar IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [Configuración de Git](./forms/developing-for-cloud-service/setup-git.md)
      + [Sincronizar IntelliJ con AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [Creación de un formulario](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [Habilitar componentes de Forms Portal](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [Incluir Cloud Services y FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [Configuración de nube según el contexto](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [Insertar en Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [Implementar en entorno de desarrollo](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [Actualización del tipo de archivo maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
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
   + Generación de documentos en AEM Forms CS{#doc-gen-formscs}
      + [Introducción](./forms/doc-gen-forms-cs/introduction.md)
      + [Crear credenciales de servicio](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Crear token JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Crear token de acceso](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Combinar datos con plantilla](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Probar la solución](./forms/doc-gen-forms-cs/test.md)
      + [Desafío](./forms/doc-gen-forms-cs/challenge.md)
   + Generación de documentos mediante API por lotes{#formscs-batch-api}
      + [Introducción](./forms/formscs-batch-api/introduction.md)
      + [Configurar el almacenamiento de Azure](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Crear configuración de lote USC](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Crear configuración de lote](./forms/formscs-batch-api/create-batch-config.md)
      + [Ejecutar lote](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Manipulación de PDF en Forms CS{#forms-cs-assembler}
      + [Introducción](./forms/forms-cs-assembler/introduction.md)
      + [Crear credenciales de servicio](./forms/forms-cs-assembler/service-credentials.md)
      + [Crear token JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Crear token de acceso](./forms/forms-cs-assembler/create-access-token.md)
      + [Archivos de PDF de ensamblado](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilidades de PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Probar la solución](./forms/forms-cs-assembler/test.md)
      + [Desafío](./forms/forms-cs-assembler/challenge.md)
   + Almacenamiento de Azure Portal{#forms-cs-azure-portal}
      + [Introducción](./forms/forms-cs-azure-portal/introduction.md)
      + [Crear modelo de datos de formulario](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Almacenar datos de formulario en Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Cumplimentar el formulario previamente](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Envíos de consultas](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Crear flujo de trabajo de revisión{#create-aem-workflow}
      + [Externalización del almacenamiento del flujo de trabajo](./forms/create-aem-workflow/externalize-workflow.md)
      + [Crear modelo de flujo de trabajo](./forms/create-aem-workflow/create-workflow.md)
      + [flujo de trabajo de déclencheur](./forms/create-aem-workflow/configure-af.md)
   + Adobe Sign con AEM Forms{#forms-and-sign}
      + [Introducción](./forms/forms-and-sign/introduction.md)
      + [Aplicación de API de Adobe Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuración en la nube de Adobe Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Crear formulario adaptable](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurar para rellenar y firmar](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integración con Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configuración de la integración](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
   + Integración con Microsoft Dynamics{#formscs-dynamics-crm}
      + [Crear aplicación de Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configurar fuentes de datos](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Crear modelo de datos de formulario](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Crear formulario adaptable](./forms/formscs-dynamics-crm/create-adaptive-form.md)
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
   + Configuración{#set-up}
      + [Aprovisionamiento de cuentas y servicios](./asset-compute/set-up/accounts-and-services.md)
      + [Entorno de desarrollo local](./asset-compute/set-up/development-environment.md)
      + [Creador de aplicaciones](./asset-compute/set-up/app-builder.md)
   + Desarrollar{#develop}
      + [Creación de un proyecto de Asset compute](./asset-compute/develop/project.md)
      + [Configuración de variables de entorno](./asset-compute/develop/environment-variables.md)
      + [Configurar manifest.yml](./asset-compute/develop/manifest.md)
      + [Desarrollo de un trabajador](./asset-compute/develop/worker.md)
      + [Uso de la herramienta de desarrollo](./asset-compute/develop/development-tool.md)
   + Prueba y depuración{#test-debug}
      + [Probar un trabajador](./asset-compute/test-debug/test.md)
      + [Depurar un trabajador](./asset-compute/test-debug/debug.md)
   + Implementación de{#deploy}
      + [Implementar en Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrar con AEM](./asset-compute/deploy/processing-profiles.md)
   + Avanzado {#advanced}
      + [Trabajadores de metadatos](./asset-compute/advanced/metadata.md)
   + [Solución de problemas](./asset-compute/troubleshooting.md)
+ Nube 5{#cloud-5}
   + [Introducción](./cloud-5/cloud5-introduction.md)
   + [Temporada 1](./cloud-5/cloud5-season-1.md)
   + [Temporada 2](./cloud-5/cloud5-season-2.md)
   + [AEM CDN Parte 1](./cloud-5/cloud5-aem-cdn-part1.md)
   + [AEM CDN Parte 2](./cloud-5/cloud5-aem-cdn-part2.md)
   + [Archivos de registro AEM](./cloud-5/cloud5-aem-log-files.md)
   + [Tokens de inicio de sesión](./cloud-5/cloud5-getting-login-token-integrations.md)
   + [Cloud Dispatcher](./cloud-5/cloud5-aem-dispatcher-cloud.md)
   + [Migración 1](./cloud-5/cloud5-aem-content-migration-part-1.md)
   + [Migración 2](./cloud-5/cloud5-aem-content-migration-part-2.md)
   + [Validador de Dispatcher](./cloud-5/cloud5-aem-dispatcher-validator.md)
   + [Buscar e indexar](./cloud-5/cloud5-aem-search-and-indexing.md)
   + [Adobe App Builder](./cloud-5/cloud5-adobe-app-builder.md)
   + Temporada 2{#season-2}
      + [Fragmentos](./cloud-5/season-2/cloud5-experience-v-content-fragments.md)
      + [Modernizador de repositorios](./cloud-5/season-2/cloud5-repo-modernizer.md)
      + [Admin Console](./cloud-5/season-2/cloud5-admin-console.md)
      + [REPOINIT](./cloud-5/season-2/cloud5-repoinit.md)
      + [Programador de trabajos de Sling](./cloud-5/season-2/cloud5-sling-job-scheduler.md)
      + [Corrección de la caché](./cloud-5/season-2/cloud5-fix-your-cache.md)
      + [Corrección de reescrituras](./cloud-5/season-2/cloud5-fix-your-rewrites.md)
      + [Cloud Manager: auditoría de experiencias](./cloud-5/season-2/cloud5-MoCM-experience-audit.md)
      + [Cloud Manager: pruebas de unidad](./cloud-5/season-2/cloud5-MoCM-unit-tests.md)
+ [AEM serie de expertos](./aem-experts-series.md)
+ Tutorials de varios pasos{#multi-step-tutorials}
   + [Desarrollo de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=es)
   + [SPA Editor (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [SPA Editor (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites y Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Autenticación basada en tokens](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
