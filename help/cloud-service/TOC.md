---
user-guide-title: Tutoriales de Adobe Experience Manager as a Cloud Service
user-guide-description: Una recopilación de tutoriales de Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriales de AEM as a Cloud Service
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: d62332374e8885e077f8227bcdec6a908c782ccc
workflow-type: tm+mt
source-wordcount: '1171'
ht-degree: 18%

---


# Tutoriales de Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Información general](./overview.md)
+ AEM {#aem-trials}
   + [Imágenes](./aem-trials/images.md)
+ AEM as a Cloud Service Introducción a la{#introduction}
   + [AEM ¿Qué está as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Arquitectura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Estrategia y liderazgo mental{#strategy}
      + [Experience Manager - Modelos y arquetipos de gobernanza y personal](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Cómo aumentar la velocidad del contenido con Adobe Experience Manager](./introduction/drive-content-velocity-for-sites.md)
      + [AEM Acelerar la velocidad del contenido con sistemas de estilos de](./introduction/accelerate-content-velocity-aem.md)
+ Integraciones de Experience Cloud{#integrations}
   + [Integraciones](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ Tecnología subyacente {#underlying-technology}
   + [AEM Arquitectura de](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Repositorio de contenido Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Servicios de creación y publicación](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [Complemento de Sidekick de AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [Programas](./cloud-manager/programs.md)
   + [Entornos](./cloud-manager/environments.md)
   + [Canalización de producción de CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Canalización de no producción de CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Actividad](./cloud-manager/activity.md)
   + [Nombres de dominio personalizados](./cloud-manager/custom-domain-names.md)
   + Operaciones de desarrollo{#devops}
      + [Implementación del código](./cloud-manager/devops/deploy-code.md)
      + [Combinar proyectos](./cloud-manager/devops/merge-projects.md)
      + [Configuración de canalizaciones](./cloud-manager/devops/configure-pipelines.md)
      + [Integración continua](./cloud-manager/devops/continuous-integration.md)
      + [Analizar resultados de pruebas](./cloud-manager/devops/analyze-test-results.md)
      + [Configuraciones de Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
+ Configuración del entorno de desarrollo local {#local-development-environment-set-up}
   + [Información general](./local-development-environment/overview.md)
   + [Herramientas de desarrollo](./local-development-environment/development-tools.md)
   + [AEM SDK local de](./local-development-environment/aem-runtime.md)
   + [Herramientas locales de Dispatcher](./local-development-environment/dispatcher-tools.md)
+ Desarrollo{#developing}
   + Extensibilidad{#extensibility}
      + Generador de aplicaciones{#app-builder}
         + [Generar token de acceso JWT](./developing/extensibility/app-builder/jwt-auth.md)
         + [Generar token de acceso de servidor a servidor](./developing/extensibility/app-builder/server-to-server-auth.md)
      + Extensibilidad de IU{#ui}
         + [Información general](./developing/extensibility/ui/overview.md)
         + [Proyecto de consola de Adobe Developer](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [Inicializar aplicación](./developing/extensibility/ui/app-initialization.md)
         + [Registrar extensión](./developing/extensibility/ui/extension-registration.md)
         + [Modal](./developing/extensibility/ui/modal.md)
         + [Acción de Adobe I/O Runtime](./developing/extensibility/ui/runtime-action.md)
         + [Verificar](./developing/extensibility/ui/verify.md)
         + [Implementación de](./developing/extensibility/ui/deploy.md)
         + Fragmentos de contenido{#content-fragments}
            + [Información general](./developing/extensibility/ui/content-fragments/overview.md)
            + Ejemplos{#examples}
               + [Generación de imágenes de IA](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [Actualización de propiedades por lotes](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [Columnas de cuadrícula personalizadas](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [Exportar como XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [Botón de barra de herramientas RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [Widgets RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [Distintivos RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
               + [Campos personalizados](./developing/extensibility/ui/content-fragments/examples/editor-custom-field.md)
   + Conceptos básicos de desarrollo{#basics}
      + [AEM SDK de](./developing/basics/aem-sdk.md)
      + [Entorno de desarrollo local](./developing/basics/local-development-environment.md)
      + [Tipo de archivo del proyecto AEM](./developing/basics/aem-project-archetype.md)
      + [Estructura del proyecto AEM](./developing/basics/project-structure.md)
      + [Contenido mutable e inmutable](./developing/basics/mutable-immutable.md)
      + [Paquete de estructura de repositorio](./developing/basics/repository-structure-package.md)
      + [Publicación de contenido](./developing/basics/content-publishing.md)
      + [Configuraciones de OSGi](./developing/basics/osgi-configurations.md)
      + [Migración de configuración de Dispatcher](./developing/basics/dispatcher-configuration.md)
   + AEM Proyectos de{#aem-projects}
      + [AEM Proyecto de Maven](./developing/projects/maven-project-structure.md)
      + [AEM Limpieza de un proyecto de Maven de](./developing/projects/remove-samples.md)
   + Servicios OSGi{#osgi-services}
      + [Conceptos básicos del servicio OSGi](./developing/osgi-services/basics.md)
      + [Ciclo de vida del componente OSGi](./developing/osgi-services/lifecycle.md)
      + [Conceptos básicos de configuraciones de OSGi](./developing/osgi-services/configurations.md)
      + [Configuraciones de OSGi mediante OCD](./developing/osgi-services/configurations-ocd.md)
   + Avanzadas{#advanced}
      + [Almacenar en caché variantes de página](./developing/advanced/variant-caching.md)
      + [Protección CSRF](./developing/advanced/csrf-protection.md)
      + [Áreas de nombres personalizadas](./developing/advanced/custom-namespaces.md)
      + [Usuarios de servicio](./developing/advanced/service-users.md)
      + [API de imagen optimizadas para la web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
   + Entorno de desarrollo rápido{#rde}
      + [Información general](./developing/rde/overview.md)
      + [Cómo realizar la configuración](./developing/rde/how-to-setup.md)
      + [Cómo usar](./developing/rde/how-to-use.md)
      + [Ciclo de vida del desarrollo](./developing/rde/development-life-cycle.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ AEM Depuración de{#debugging}
   + AEM Depuración del SDK de{#debugging-aem-sdk}
      + [Información general](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registros](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuración remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Consola web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Herramientas de Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Otras herramientas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + AEM Depuración as a Cloud Service de{#debugging-aem-as-a-cloud-service}
      + [Información general](./debugging/cloud-service/overview.md)
      + [Registros](./debugging/cloud-service/logs.md)
      + [Creación e implementación](./debugging/cloud-service/build-and-deployment.md)
      + [Consola de desarrollador](./debugging/cloud-service/developer-console.md)
      + [Explorador del repositorio](./debugging/cloud-service/repository-browser.md)
      + Riesgos{#risks}
         + [Advertencias transversales](./debugging/cloud-service/risks/traversals.md)
+ Entrega de contenido{#content-delivery}
   + [Redirecciones de URL](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ Almacenamiento en caché{#caching}
   + [Información general](./caching/overview.md)
   + [AEM Servicio de publicación de](./caching/publish.md)
   + [AEM Servicio de autor de](./caching/author.md)
   + [Análisis del índice de aciertos de caché CDN](./caching/cdn-cache-hit-ratio-analysis.md)
   + Cómo:{#how-to}
      + [Habilitar almacenamiento en caché](./caching/how-to/enable-caching.md)
      + [Deshabilitar almacenamiento en caché](./caching/how-to/disable-caching.md)
+ AEM Acceder a{#accessing}
   + [Información general](./accessing/overview.md)
   + [Usuarios de Adobe IMS](./accessing/adobe-ims-users.md)
   + [Grupos de usuarios de IMS de Adobe](./accessing/adobe-ims-user-groups.md)
   + [Perfiles del producto IMS de Adobe](./accessing/adobe-ims-product-profiles.md)
   + [AEM usuarios, grupos y permisos de la](./accessing/aem-users-groups-and-permissions.md)
   + [Configuración del acceso a la guía de AEM](./accessing/walk-through.md)
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
      + [Conexiones SQL con DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Conexiones SQL con API de SQL Java](./networking/examples/sql-java-apis.md)
      + [Servicio de correo electrónico](./networking/examples/email-service.md)
+ Seguridad {#security}
   + Reglas de filtro de tráfico, incluidas las reglas WAF{#traffic-filter-and-waf-rules}
      + [Información general](./security/traffic-filter-rules/overview.md)
      + [Cómo realizar la configuración](./security/traffic-filter-rules/how-to-setup.md)
      + [Ejemplos y análisis de resultados](./security/traffic-filter-rules/examples-and-analysis.md)
      + [Prácticas recomendadas](./security/traffic-filter-rules/best-practices.md)
+ AEM Eventing{#aem-eventing}
   + [Información general](./eventing/overview.md)
   + Ejemplos{#examples}
      + [AEM Webhook: Recibir eventos](./eventing/examples/webhook.md)
      + [AEM Diario: Cargar eventos de](./eventing/examples/journaling.md)
      + [Acción De Adobe I/O Runtime AEM: Recibir Eventos De](./eventing/examples/runtime-action.md)
      + [Acción de Adobe I/O Runtime AEM: eventos de proceso de](./eventing/examples/event-processing-using-runtime-action.md)
      + [Eventos de AEM Assets: integración de PIM](./eventing/examples/assets-pim-integration.md)
+ Migración {#migration}
   + [Herramienta de transferencia de contenido](./migration/content-transfer-tool.md)
   + [Importación masiva de recursos](./migration/bulk-import.md)
   + El paso de AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Introducción](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Incorporación](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA y CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM Herramientas de modernización de](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernización del repositorio](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microservicios de Asset compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Búsqueda e indexación](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migración de contenido {#content-migration}
         + [Servicio de importación por lotes](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Herramienta de transferencia de contenido](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [Preguntas más frecuentes](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
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
      + [Modernizador de repositorio de código](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Conversor de índices](./migration/cloud-acceleration-manager/index-converter.md)
      + [Herramienta de migración del flujo de trabajo de recursos](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navegación por Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Uso de Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [Fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ Forms{#forms}
   + Desarrollo para Forms as a Cloud Service{#developing-for-cloud-service}
      + [1 - Introducción](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - Instalar IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - Configurar Git](./forms/developing-for-cloud-service/setup-git.md)
      + [AEM 4 - Sincronizar IntelliJ con la opción de sincronización con la](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 - Crear un formulario](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - Controlador de envío personalizado](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - Registro del servlet mediante el tipo de recurso](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 - Habilitar los componentes del portal de Forms](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 - Incluir Cloud Service y FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [10 - Configuración de nube según el contexto](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [11 - Insertar en Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [12 - Implementar en el entorno de desarrollo](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [13 - Actualización del arquetipo de Maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + Crear formulario adaptable{#create-first-af}
      + [Introducción](./forms/create-first-af/introduction.md)
      + [Crear tema](./forms/create-first-af/create-theme.md)
      + [Crear plantilla](./forms/create-first-af/create-template.md)
      + [Crear fragmento](./forms/create-first-af/create-fragments.md)
      + [Crear formulario](./forms/create-first-af/create-af.md)
      + [Configuración del panel raíz](./forms/create-first-af/configure-root-panel.md)
      + [Configurar el panel Personas](./forms/create-first-af/configure-people-panel.md)
      + [Configurar el panel de ingresos](./forms/create-first-af/configure-income-panel.md)
      + [Configurar el panel de recursos](./forms/create-first-af/configure-assets-panel.md)
      + [Configurar el panel de inicio](./forms/create-first-af/configure-start-panel.md)
      + [Agregar y configurar la barra de herramientas](./forms/create-first-af/add-configure-toolbar.md)
   + Servicio de envío personalizado con formulario sin encabezado{#custom-submit-headless-forms}
      + [1 - Introducción](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - Crear un servicio de envío personalizado](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - Mostrar la respuesta](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + AEM Forms y Analytics{#forms-and-analytics}
      + [Introducción](./forms/form-data-analytics/introduction.md)
      + [Crear elementos de datos](./forms/form-data-analytics/data-elements.md)
      + [Crear reglas](./forms/form-data-analytics/rules.md)
      + [Prueba de la solución](./forms/form-data-analytics/test.md)
   + Generación de documentos en AEM Forms CS{#doc-gen-formscs}
      + [Introducción](./forms/doc-gen-forms-cs/introduction.md)
      + [Crear credenciales de servicio](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Crear token JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Crear token de acceso](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Combinar datos con plantilla](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Prueba de la solución](./forms/doc-gen-forms-cs/test.md)
      + [Desafío](./forms/doc-gen-forms-cs/challenge.md)
   + Generación de documentos mediante API por lotes{#formscs-batch-api}
      + [Introducción](./forms/formscs-batch-api/introduction.md)
      + [Configurar el almacenamiento de Azure](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Crear configuración de lote USC](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Crear configuración por lotes](./forms/formscs-batch-api/create-batch-config.md)
      + [Ejecutar lote](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Manipulación de PDF en Forms CS{#forms-cs-assembler}
      + [Introducción](./forms/forms-cs-assembler/introduction.md)
      + [Crear credenciales de servicio](./forms/forms-cs-assembler/service-credentials.md)
      + [Crear token JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Crear token de acceso](./forms/forms-cs-assembler/create-access-token.md)
      + [Montar archivos de PDF](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilidades de PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Prueba de la solución](./forms/forms-cs-assembler/test.md)
      + [Desafío](./forms/forms-cs-assembler/challenge.md)
   + Almacenar envíos de formularios con etiquetas de índice de blobs{#store-submiited-data-with-metadata-tags}
      + [Introducción](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [Ampliar componente de grupo de opciones](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [Crear configuración de OSGi](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [Creación de etiquetas de índice](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [Crear envío personalizado](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + Rellenar previamente formulario basado en componentes principales{#prefill-core-component-based-form}
      + [Introducción](./forms/prefill-core-component-form/introduction.md)
      + [Escribir servicio de relleno previo](./forms/prefill-core-component-form/pre-fill-service.md)
      + [Prueba de la solución](./forms/prefill-core-component-form/test-solution.md)
   + Azure Portal Storage{#forms-cs-azure-portal}
      + [Introducción](./forms/forms-cs-azure-portal/introduction.md)
      + [Crear modelo de datos de formulario](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Almacenar datos de formulario en Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Rellenar previamente formulario](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Envíos de consultas](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Guardar y reanudar el rellenado de formularios{#prefill-azure-storage}
      + [1- Introducción](./forms/prefill-azure-storage/introduction.md)
      + [2- Crear componente de página](./forms/prefill-azure-storage/page-component.md)
      + [3- Crear una plantilla de formulario adaptable](./forms/prefill-azure-storage/associate-page-component.md)
      + [4- Crear la integración de Azure Storage](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - Crear la integración de SendGrid](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - Crear el formulario adaptable](./forms/prefill-azure-storage/create-af.md)
      + [7 - Implementar los recursos de muestra](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + Crear flujo de trabajo de revisión{#create-aem-workflow}
      + [Externalización del almacenamiento de flujo de trabajo](./forms/create-aem-workflow/externalize-workflow.md)
      + [Crear modelo de flujo de trabajo](./forms/create-aem-workflow/create-workflow.md)
      + [flujo de trabajo de déclencheur](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign con AEM Forms{#forms-and-sign}
      + [Introducción](./forms/forms-and-sign/introduction.md)
      + [Aplicación API de Acrobat Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuración de nube de Acrobat Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Crear formulario adaptable](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurar para rellenar y firmar](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integración con Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configuración de la integración](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [Analizar datos de formularios enviados](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [Enviar documento de registro como archivo adjunto de correo electrónico](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [Extracción de datos adjuntos de formularios enviados](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Integración con Microsoft Dynamics{#formscs-dynamics-crm}
      + [Crear aplicación de Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configurar fuente de datos](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Crear modelo de datos de formulario](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Crear formulario adaptable](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Integración con Salesforce{#integrate-with-salesforce}
      + [Introducción](./forms/integrate-with-salesforce/introduction.md)
      + [Crear aplicación conectada](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Crear archivo swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [Crear fuente de datos](./forms/integrate-with-salesforce/create-data-source.md)
      + [Creación de un modelo de datos de formulario](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [Probar el envío del formulario](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [Evento de clic de prueba](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + Almacenar envíos de formularios en una unidad y SharePoint{#one-drive}
      + [Almacenar datos de formulario en una unidad](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [Almacenar datos de formulario en SharePoint](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [Rellenar previamente el formulario con datos de la lista de SharePoint](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [Inserción de datos en la lista de SharePoint mediante un flujo de trabajo](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Extensibilidad de asset compute{#asset-compute}
   + [Información general](./asset-compute/overview.md)
   + Configuración de{#set-up}
      + [Aprovisionamiento de cuentas y servicios](./asset-compute/set-up/accounts-and-services.md)
      + [Entorno de desarrollo local](./asset-compute/set-up/development-environment.md)
      + [Generador de aplicaciones](./asset-compute/set-up/app-builder.md)
   + Desarrollar{#develop}
      + [Creación de un proyecto de Asset compute](./asset-compute/develop/project.md)
      + [Configuración de variables de entorno](./asset-compute/develop/environment-variables.md)
      + [Configuración de manifest.yml](./asset-compute/develop/manifest.md)
      + [Desarrollo de un trabajador](./asset-compute/develop/worker.md)
      + [Uso de la herramienta de desarrollo](./asset-compute/develop/development-tool.md)
   + Prueba y depuración{#test-debug}
      + [Prueba de un trabajador](./asset-compute/test-debug/test.md)
      + [Depuración de un trabajador](./asset-compute/test-debug/debug.md)
   + Implementar{#deploy}
      + [Implementación en Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [AEM Integrar con el](./asset-compute/deploy/processing-profiles.md)
   + Avanzadas{#advanced}
      + [Trabajadores de metadatos](./asset-compute/advanced/metadata.md)
   + [Solución de problemas](./asset-compute/troubleshooting.md)

+ Tutorials de varios pasos{#multi-step-tutorials}
   + [Desarrollo de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=es){target=_blank}
   + [SPA Editor de (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM SITES y ADOBE TARGET](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [Autenticación basada en tokens](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ Recursos de expertos {#expert-resources}
   + AEM Campeones de {#aem-champions}
      + [Guía de incorporación de Cloud Manager](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Tipos de entorno de Cloud Manager](./expert-resources/aem-champions/environment-types.md)
      + [IU de Cloud Manager](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM Serie de expertos de](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [Introducción](./expert-resources/cloud-5/cloud5-introduction.md)
      + [Temporada 1](./expert-resources/cloud-5/cloud5-season-1.md)
      + [Temporada 2](./expert-resources/cloud-5/cloud5-season-2.md)
      + [Temporada 3](./expert-resources/cloud-5/cloud5-season-3.md)
      + [AEM CDN CDN, parte 1](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN CDN, parte 2](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [AEM Archivos de registro de](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [Tokens de inicio de sesión](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Cloud Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [Migración 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Migración 2](./expert-resources/cloud-5/cloud5-aem-content-migration-part-2.md)
      + [Validador de Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [Búsqueda e indexación](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + Temporada 2{#season-2}
         + [Fragmentos](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Modernizador de repositorios](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Planificador de trabajos de Sling](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [Reparar la caché](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [Corregir las reescrituras](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager: auditoría de experiencias](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager: pruebas unitarias](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager: Pruebas funcionales](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + Temporada 3{#season-3}
         + [Búsqueda de terceros](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
         + [Monitorización de usuarios reales (RUM)](./expert-resources/cloud-5/season-3/cloud5-rum.md)
         + [Trabajadores de Edge](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [Publicar, cancelar la publicación de eventos en Edge Delivery Services](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [Índices de consulta y fórmulas de Excel](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [Traiga su propia CDN de Cloudflare](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [Integrar AEM Assets](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
