---
user-guide-title: Tutoriales de Adobe Experience Manager as a Cloud Service
user-guide-description: Una recopilación de tutoriales de Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriales de AEM as a Cloud Service
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: 99aa43460a76460175123a5bfe5138767491252b
workflow-type: tm+mt
source-wordcount: '1366'
ht-degree: 16%

---


# Tutoriales de Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Información general](./overview.md)
+ AEM {#aem-trials}
   + [Imágenes](./aem-trials/images.md)
+ Listas de reproducción{#playlists}
   + [AEM desarrollo de la](./playlists/development.md)
+ Introducción a AEM as a Cloud Service{#introduction}
   + [¿Qué es AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Arquitectura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Estrategia y liderazgo mental{#strategy}
      + [Experience Manager - Modelos y arquetipos de gobernanza y personal](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Cómo aumentar la velocidad del contenido con Adobe Experience Manager](./introduction/drive-content-velocity-for-sites.md)
+ integraciones de Experience Cloud{#integrations}
   + [Integraciones](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ Tecnología subyacente {#underlying-technology}
   + [AEM Arquitectura de](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Repositorio de contenido Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Creación y servicios de Publish](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [Complemento de Sidekick de AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [Programas](./cloud-manager/programs.md)
   + [Entornos](./cloud-manager/environments.md)
   + [Uso de un repositorio de GitHub](./cloud-manager/byogithub.md)
   + [Canalización de producción de CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Canalización de no producción de CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Actividad](./cloud-manager/activity.md)
   + [Nombres de dominio personalizados](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names){target=_blank}
   + Operaciones de desarrollo {#devops}
      + [Implementación del código](./cloud-manager/devops/deploy-code.md)
      + [Combinar proyectos](./cloud-manager/devops/merge-projects.md)
      + [Configuración de canalizaciones](./cloud-manager/devops/configure-pipelines.md)
      + [Integración continua](./cloud-manager/devops/continuous-integration.md)
      + [Analizar resultados de pruebas](./cloud-manager/devops/analyze-test-results.md)
      + [Configuraciones de Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [Análisis de registro de CDN](./cloud-manager/devops/cdn-log-analysis.md)
+ Configuración del entorno de desarrollo local {#local-development-environment-set-up}
   + [Información general](./local-development-environment/overview.md)
   + [Herramientas de desarrollo](./local-development-environment/development-tools.md)
   + [AEM SDK local](./local-development-environment/aem-runtime.md)
   + [Herramientas locales de Dispatcher](./local-development-environment/dispatcher-tools.md)
+ Desarrollando{#developing}
   + Extensibilidad{#extensibility}
      + App Builder{#app-builder}
         + [Generar token de acceso JWT](./developing/extensibility/app-builder/jwt-auth.md)
         + [Generar token de acceso de servidor a servidor](./developing/extensibility/app-builder/server-to-server-auth.md)
         + [Verificación del webhook de Github](./developing/extensibility/app-builder/github-webhook-verification.md)
      + Extensibilidad de la interfaz de usuario {#ui}
         + [Información general](./developing/extensibility/ui/overview.md)
         + [Proyecto de Adobe Developer Console](./developing/extensibility/ui/adobe-developer-console-project.md)
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
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [Entorno de desarrollo local](./developing/basics/local-development-environment.md)
      + [Tipo de archivo del proyecto AEM](./developing/basics/aem-project-archetype.md)
      + [Estructura del proyecto AEM](./developing/basics/project-structure.md)
      + [Contenido mutable e inmutable](./developing/basics/mutable-immutable.md)
      + [Paquete de estructura de repositorio](./developing/basics/repository-structure-package.md)
      + [Publicación de contenido](./developing/basics/content-publishing.md)
      + [Configuraciones de OSGi](./developing/basics/osgi-configurations.md)
      + [Migración de configuración de Dispatcher](./developing/basics/dispatcher-configuration.md)
   + AEM Proyectos {#aem-projects}
      + [AEM Proyecto de Maven](./developing/projects/maven-project-structure.md)
      + [AEM Limpieza de un proyecto de Maven de](./developing/projects/remove-samples.md)
   + Servicios OSGi{#osgi-services}
      + [Conceptos básicos del servicio OSGi](./developing/osgi-services/basics.md)
      + [Ciclo de vida del componente OSGi](./developing/osgi-services/lifecycle.md)
      + [Conceptos básicos de configuraciones de OSGi](./developing/osgi-services/configurations.md)
      + [Configuraciones de OSGi mediante OCD](./developing/osgi-services/configurations-ocd.md)
   + Avanzado{#advanced}
      + [Variantes de página en caché](./developing/advanced/variant-caching.md)
      + [Protección CSRF](./developing/advanced/csrf-protection.md)
      + [Áreas de nombres personalizadas](./developing/advanced/custom-namespaces.md)
      + [Parametrizar modelos Sling de HTL](./developing/advanced/sling-model-parameters.md)
      + [Secretos](./developing/advanced/secrets.md)
      + [Usuarios del servicio](./developing/advanced/service-users.md)
      + [API de imagen optimizadas para la web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [AEM Ejecutar el trabajo en la instancia de líder en el autor de](./developing/advanced/run-job-on-leader-instance-in-aem-author.md)
   + Entorno de desarrollo rápido{#rde}
      + [Información general](./developing/rde/overview.md)
      + [Cómo realizar la configuración](./developing/rde/how-to-setup.md)
      + [Cómo usar](./developing/rde/how-to-use.md)
      + [Ciclo de vida del desarrollo](./developing/rde/development-life-cycle.md)
   + Editor universal{#universal-editor}
      + Edición de aplicación de React{#react-app-editing}
         + [Información general](./developing/universal-editor/react-app/overview.md)
         + [Configuración de desarrollo local](./developing/universal-editor/react-app/local-development-setup.md)
         + [Aplicación React de Instrumento](./developing/universal-editor/react-app/instrument-to-edit-content.md)
   + AEM [API de SDK JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ AEM Depuración de {#debugging}
   + AEM Depurando el SDK {#debugging-aem-sdk} de la
      + [Información general](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registros](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuración remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Consola web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Herramientas de Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Otras herramientas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depurando AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Información general](./debugging/cloud-service/overview.md)
      + [Registros](./debugging/cloud-service/logs.md)
      + [Creación e implementación](./debugging/cloud-service/build-and-deployment.md)
      + [Consola de desarrollador](./debugging/cloud-service/developer-console.md)
      + [Explorador del repositorio](./debugging/cloud-service/repository-browser.md)
      + Riesgos{#risks}
         + [Advertencias transversales](./debugging/cloud-service/risks/traversals.md)
+ AEM API de{#aem-apis}
   + [Información general](./apis/overview.md)
   + [AEM API de datos basadas en API de OpenAPI (servidor a servidor)](./apis/invoke-openapi-based-aem-apis.md)
   + [AEM API basadas en API de OpenAPI (autenticación de usuario)](./apis/invoke-openapi-based-aem-apis-from-web-app.md)
+ Entrega de contenido{#content-delivery}
   + [Nombre de dominio personalizado](./content-delivery/custom-domain-names.md)
   + [Nombre de dominio personalizado con CDN administrada por Adobe](./content-delivery/custom-domain-name-with-adobe-managed-cdn.md)
   + [Nombre de dominio personalizado con CDN del cliente](./content-delivery/custom-domain-names-with-customer-managed-cdn.md)
   + [Almacenamiento en caché](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/overview){target=_blank}
   + [CDN de Adobe - más allá del almacenamiento en caché](./content-delivery/adobe-cdn-beyond-caching.md)
   + [Páginas de error personalizadas](./content-delivery/custom-error-pages.md)
   + [Redirecciones de URL](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html){target=_blank}
+ Almacenando en caché {#caching}
   + [Información general](./caching/overview.md)
   + [AEM servicio de Publish de](./caching/publish.md)
   + [AEM Servicio de autor de](./caching/author.md)
   + [Análisis del índice de aciertos de caché CDN](./caching/cdn-cache-hit-ratio-analysis.md)
   + Cómo {#how-to}
      + [Habilitar almacenamiento en caché](./caching/how-to/enable-caching.md)
      + [Deshabilitar almacenamiento en caché](./caching/how-to/disable-caching.md)
      + [Purgar caché](./caching/how-to/purge-cache.md)
+ AEM Accediendo a {#accessing}
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
   + [Bloqueo de ataques DoS/DDoS mediante reglas de filtro de tráfico](./security/blocking-dos-attack-using-traffic-filter-rules.md)
   + Reglas de filtro de tráfico, incluidas las reglas de WAF{#traffic-filter-and-waf-rules}
      + [Información general](./security/traffic-filter-rules/overview.md)
      + [Cómo realizar la configuración](./security/traffic-filter-rules/how-to-setup.md)
      + [Ejemplos y análisis de resultados](./security/traffic-filter-rules/examples-and-analysis.md)
      + [Prácticas recomendadas](./security/traffic-filter-rules/best-practices.md)
+ AEM Eventing de la{#aem-eventing}
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
      + [Microservicios de Asset Compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
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
      + [Herramientas de refactorización de código](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernizador de repositorio de código](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher Converter](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Conversor de índices](./migration/cloud-acceleration-manager/index-converter.md)
      + [Herramienta de migración del flujo de trabajo de recursos](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navegación por Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Uso de Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [Fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html){target=_blank}
+ Forms{#forms}
   + Desarrollando para Forms as a Cloud Service{#developing-for-cloud-service}
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
   + Servicio de envío personalizado con formulario sin encabezado {#custom-submit-headless-forms}
      + [1 - Introducción](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - Crear un servicio de envío personalizado](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - Mostrar la respuesta](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + Crear componente de bloque de direcciones {#create-address-block}
      + [1 - Introducción](./forms/create-address-block-component/introduction.md)
      + [2 - Configuración](./forms/create-address-block-component/set-up.md)
      + [3 - Crear componente](./forms/create-address-block-component/creating-address-component.md)
      + [4 - Implementar el componente](./forms/create-address-block-component/deploy-your-project.md)
   + Crear componente de imagen en la que se puede hacer clic{#clickable-image-component}
      + [1 - Introducción](./forms/clickable-image-component/introduction.md)
      + [2 - Crear componente](./forms/clickable-image-component/create-component.md)
      + [3 - Controlar el evento de clic](./forms/clickable-image-component/handle-click-event.md)
   + AEM Forms y Analytics{#forms-and-analytics}
      + [Introducción](./forms/form-data-analytics/introduction.md)
      + [Crear elementos de datos](./forms/form-data-analytics/data-elements.md)
      + [Crear reglas](./forms/form-data-analytics/rules.md)
      + [Prueba de la solución](./forms/form-data-analytics/test.md)
   + Creando componente desplegable de países {#countries-drop-down}
      + [Introducción](./forms/countries-drop-down/introduction.md)
      + [Crear componente](./forms/countries-drop-down/component.md)
      + [Crear cuadro de diálogo](./forms/countries-drop-down/dialog.md)
      + [Crear modelo de Sling](./forms/countries-drop-down/slingmodel.md)
      + [Generar y probar](./forms/countries-drop-down/build.md)
   + Creando variaciones de botón{#style-system}
      + [Introducción](./forms/style-system/introduction.md)
      + [Definir directiva](./forms/style-system/style-policy.md)
      + [Definir variaciones](./forms/style-system/create-variations.md)
      + [Variaciones de prueba](./forms/style-system/build.md)
   + Usando fichas verticales{#using-vertical-tabs}
      + [1. Introducción](./forms/using-vertical-tabs/introduction.md)
      + [2. Crear formulario](./forms/using-vertical-tabs/create-af.md)
      + [3. Navegación](./forms/using-vertical-tabs/navigation.md)
      + [4. Añadir iconos](./forms/using-vertical-tabs/icons.md)
   + Usando salida y servicio de formularios{#forms-cs-output-and-forms-service}
      + [Generar PDF](./forms/forms-cs-output-and-forms-service/outputservice.md)
   + Generación de documentos en AEM Forms CS{#doc-gen-formscs}
      + [Introducción](./forms/doc-gen-forms-cs/introduction.md)
      + [Crear credenciales de servicio](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Crear token JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Crear token de acceso](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Combinar datos con plantilla](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Prueba de la solución](./forms/doc-gen-forms-cs/test.md)
      + [Desafío](./forms/doc-gen-forms-cs/challenge.md)
   + Usando API de DocAssurance {#doc-assurance-api}
      + [Fragmentos de código de ejemplo](./forms/doc-assurance-api/using-doc-assurance-api.md)
   + Generación de documentos mediante la API por lotes {#formscs-batch-api}
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
   + Integrar con Marketo{#froms-cs-with-marketo}
      + [Introducción](./forms/forms-cs-with-marketo/part1.md)
      + [Crear Source de datos](./forms/forms-cs-with-marketo/part2.md)
      + [Crear modelo de datos de formulario](./forms/forms-cs-with-marketo/part3.md)
   + Almacenar envíos de formularios con etiquetas de índice de blobs{#store-submiited-data-with-metadata-tags}
      + [Introducción](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [Ampliar componente de grupo de opciones](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [Crear configuración de OSGi](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [Creación de etiquetas de índice](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [Crear envío personalizado](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + Rellenar previamente formulario basado en componente principal {#prefill-core-component-based-form}
      + [Introducción](./forms/prefill-core-component-form/introduction.md)
      + [Escribir servicio de relleno previo](./forms/prefill-core-component-form/pre-fill-service.md)
      + [Prueba de la solución](./forms/prefill-core-component-form/test-solution.md)
   + Almacenamiento de Azure Portal{#forms-cs-azure-portal}
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
   + Integrar con Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configuración de la integración](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [Analizar datos de formularios enviados](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [Enviar documento de registro como archivo adjunto de correo electrónico](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [Extracción de datos adjuntos de formularios enviados](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Integrar con Microsoft Dynamics{#formscs-dynamics-crm}
      + [Crear aplicación de Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configuración de Data Source](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Crear modelo de datos de formulario](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Crear formulario adaptable](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Integrar con Salesforce{#integrate-with-salesforce}
      + [Introducción](./forms/integrate-with-salesforce/introduction.md)
      + [Crear aplicación conectada](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Crear archivo swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [Crear fuente de datos](./forms/integrate-with-salesforce/create-data-source.md)
      + [Creación de un modelo de datos de formulario](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [Probar el envío del formulario](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [Evento de clic de prueba](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + Almacenar envíos de formularios en una unidad y en SharePoint{#one-drive}
      + [Almacenar datos de formulario en una unidad](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [Almacenar datos de formulario en SharePoint](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [Rellenar previamente el formulario con datos de la lista de SharePoint](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [Inserción de datos en la lista de SharePoint mediante un flujo de trabajo](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Extensibilidad de la asset compute {#asset-compute}
   + [Información general](./asset-compute/overview.md)
   + Configurar {#set-up}
      + [Aprovisionamiento de cuentas y servicios](./asset-compute/set-up/accounts-and-services.md)
      + [Entorno de desarrollo local](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + Desarrollar {#develop}
      + [Creación de un proyecto de Asset compute](./asset-compute/develop/project.md)
      + [Configuración de variables de entorno](./asset-compute/develop/environment-variables.md)
      + [Configuración de manifest.yml](./asset-compute/develop/manifest.md)
      + [Desarrollo de un trabajador](./asset-compute/develop/worker.md)
      + [Uso de la herramienta de desarrollo](./asset-compute/develop/development-tool.md)
   + Prueba y depuración{#test-debug}
      + [Prueba de un trabajador](./asset-compute/test-debug/test.md)
      + [Depuración de un trabajador](./asset-compute/test-debug/debug.md)
   + Implementar {#deploy}
      + [Implementación en Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [AEM Integrar con el](./asset-compute/deploy/processing-profiles.md)
   + Avanzado{#advanced}
      + [Trabajadores de metadatos](./asset-compute/advanced/metadata.md)
   + [Solución de problemas](./asset-compute/troubleshooting.md)

+ Tutorials de varios pasos{#multi-step-tutorials}
   + [Desarrollo de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=es){target=_blank}
   + SPA [Editor de (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites y Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html){target=_blank}
   + [Autenticación basada en token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html){target=_blank}
+ Recursos de expertos {#expert-resources}
   + AEM Campeones de la {#aem-champions}
      + [Guía de incorporación de Cloud Manager](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Tipos de entornos de Cloud Manager](./expert-resources/aem-champions/environment-types.md)
      + [IU de Cloud Manager](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM Serie de expertos de](./expert-resources/expert-series/aem-experts-series.md)
   + Nube 5{#cloud-5}
      + [Introducción](./expert-resources/cloud-5/cloud5-introduction.md)
      + [Temporada 4](./expert-resources/cloud-5/cloud5-season-4.md)
      + [Temporada 3](./expert-resources/cloud-5/cloud5-season-3.md)
      + [Temporada 2](./expert-resources/cloud-5/cloud5-season-2.md)
      + [Temporada 1](./expert-resources/cloud-5/cloud5-season-1.md)
      + [AEM CDN CDN, parte 1](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN CDN, parte 2](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [AEM Archivos de registro de](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [Tokens de inicio de sesión](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Cloud Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [Migración 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Dispatcher Validator](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
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
         + [Trabajadores de Edge](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [Publish, cancelar la publicación de eventos en Edge Delivery Services](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [Índices de consulta y fórmulas de Excel](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [Traiga su propia CDN de Cloudflare](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [Integración de AEM Assets](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
         + [IA generativa para AEM Sites](./expert-resources/cloud-5/season-3/cloud5-generative-ai-for-aem-sites.md)
         + [Exploración del editor universal](./expert-resources/cloud-5/season-3/cloud5-exploring-universal-editor.md)
         + [Importar sitios](./expert-resources/cloud-5/season-3/cloud5-import-sites-to-edge-delivery-services.md)
         + [Uso de la API de administración](./expert-resources/cloud-5/season-3/cloud5-using-admin-api.md)
         + [Optimización de la puntuación de Lighthouse - Parte1](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part1.md)
         + [Optimización de la puntuación de Lighthouse - Parte 2](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part2.md)
         + [Optimización de la puntuación de Lighthouse - Parte3](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part3.md)
      + Temporada 4{#season-4}
         + [Prácticas recomendadas](./expert-resources/cloud-5/season-4/cloud5-edge-delivery-services-best-practices.md)
         + [Optimizaciones de búsqueda](./expert-resources/cloud-5/season-4/cloud5-search-optimization.md)
         + [Mapas de Google](./expert-resources/cloud-5/season-4/cloud5-google-maps.md)
