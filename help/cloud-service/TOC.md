---
user-guide-title: Tutoriales de Adobe Experience Manager as a Cloud Service
user-guide-description: Una recopilación de tutoriales de Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriales de AEM as a Cloud Service
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Experience Manager as a Cloud Service
team: TM
source-git-commit: 96719e9f53324469927eee83840356833b5605a0
workflow-type: tm+mt
source-wordcount: '1420'
ht-degree: 99%

---


# Tutoriales de Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Información general](./overview.md)
+ Pruebas de AEM {#aem-trials}
   + [Imágenes](./aem-trials/images.md)
+ Listas de reproducción{#playlists}
   + [Desarrollo de AEM](./playlists/development.md)
+ Introducción a AEM as a Cloud Service{#introduction}
   + [¿Qué es AEM as a Cloud Service?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Arquitectura](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Estrategia y liderazgo intelectual{#strategy}
      + [Experience Manager: modelos y arquetipos de control y dotación de personal](./introduction/experience-manager-governance-and-staffing-models.md)
+ [Experience Hub](./experience-hub.md)
+ IA {#ai}
   + [Información general](./ai/overview.md)
   + [Configuración y aprovisionamiento](./ai/setup.md)
   + [Asistente de IA](./ai/ai-assistant.md)
   + [Agentes](./ai/agents-in-aem.md)
+ Integraciones de Experience Cloud{#integrations}
   + [Integraciones](./integrations/experience-cloud.md)
   + [AEM sin encabezado y Target](./integrations/target.md)
+ Tecnología subyacente {#underlying-technology}
   + [Arquitectura de AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Repositorio de contenido Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Servicios de Author y Publish](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [Complemento de Sidekick para AEM Assets](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html?lang=es){target=_blank}
+ Cloud Manager {#cloud-manager}
   + [Programas](./cloud-manager/programs.md)
   + [Entornos](./cloud-manager/environments.md)
   + [Uso de un repositorio de GitHub](./cloud-manager/byogithub.md)
   + [Canalización de producción de CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Canalización de no producción de CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Actividad](./cloud-manager/activity.md)
   + [Nombres de dominio personalizados](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names){target=_blank}
   + [Restauración de contenido](./cloud-manager/content-restore.md)
   + Operaciones de desarrollo{#devops}
      + [Implementación de código](./cloud-manager/devops/deploy-code.md)
      + [Combinar proyectos](./cloud-manager/devops/merge-projects.md)
      + [Configuración de canalizaciones](./cloud-manager/devops/configure-pipelines.md)
      + [Integración continua](./cloud-manager/devops/continuous-integration.md)
      + [Análisis de los resultados de la prueba](./cloud-manager/devops/analyze-test-results.md)
      + [Configuraciones de Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [Análisis de registro de CDN](./cloud-manager/devops/cdn-log-analysis.md)
+ Configuración del entorno de desarrollo local {#local-development-environment-set-up}
   + [Información general](./local-development-environment/overview.md)
   + [Herramientas de desarrollo](./local-development-environment/development-tools.md)
   + [SDK de AEM local](./local-development-environment/aem-runtime.md)
   + [Herramientas locales de Dispatcher](./local-development-environment/dispatcher-tools.md)
+ Desarrollo{#developing}
   + Extensibilidad{#extensibility}
      + App Builder{#app-builder}
         + [Generar token de acceso JWT](./developing/extensibility/app-builder/jwt-auth.md)
         + [Generar token de acceso de servidor a servidor](./developing/extensibility/app-builder/server-to-server-auth.md)
         + [Verificación del webhook de Github](./developing/extensibility/app-builder/github-webhook-verification.md)
      + Extensibilidad de la IU{#ui}
         + [Información general](./developing/extensibility/ui/overview.md)
         + [Proyecto de Adobe Developer Console](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [Inicializar aplicación](./developing/extensibility/ui/app-initialization.md)
         + [Extensión de registro](./developing/extensibility/ui/extension-registration.md)
         + [Modal](./developing/extensibility/ui/modal.md)
         + [Acción de Adobe I/O Runtime](./developing/extensibility/ui/runtime-action.md)
         + [Verificar](./developing/extensibility/ui/verify.md)
         + [Implementación](./developing/extensibility/ui/deploy.md)
         + Fragmentos de contenido{#content-fragments}
            + [Información general](./developing/extensibility/ui/content-fragments/overview.md)
            + Ejemplos{#examples}
               + [Generación de imágenes de IA](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [Actualización masiva de propiedades](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [Columnas de cuadrícula personalizadas](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [Exportar como XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [Botón de barra de herramientas del RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [Widgets de RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [Distintivos de RTE](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
               + [Campos personalizados](./developing/extensibility/ui/content-fragments/examples/editor-custom-field.md)
   + Conceptos básicos de desarrollo{#basics}
      + [SDK de AEM](./developing/basics/aem-sdk.md)
      + [Entorno de desarrollo local](./developing/basics/local-development-environment.md)
      + [Arquetipo del proyecto AEM](./developing/basics/aem-project-archetype.md)
      + [Estructura del proyecto AEM](./developing/basics/project-structure.md)
      + [Contenido mutable frente a contenido inmutable](./developing/basics/mutable-immutable.md)
      + [Paquete de estructura de repositorio](./developing/basics/repository-structure-package.md)
      + [Publicación de contenido](./developing/basics/content-publishing.md)
      + [Configuraciones de OSGi](./developing/basics/osgi-configurations.md)
      + [Migración de la configuración de Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Proyectos AEM{#aem-projects}
      + [Proyecto Maven de AEM](./developing/projects/maven-project-structure.md)
      + [Limpieza de un proyecto Maven de AEM](./developing/projects/remove-samples.md)
   + Servicios OSGi{#osgi-services}
      + [Conceptos básicos del servicio OSGi](./developing/osgi-services/basics.md)
      + [Ciclo de vida del componente OSGi](./developing/osgi-services/lifecycle.md)
      + [Conceptos básicos de configuraciones de OSGi](./developing/osgi-services/configurations.md)
      + [Configuraciones de OSGi mediante OCD](./developing/osgi-services/configurations-ocd.md)
   + Avanzado{#advanced}
      + [Variantes de página en almacenamiento en caché](./developing/advanced/variant-caching.md)
      + [Protección CSRF](./developing/advanced/csrf-protection.md)
      + [Espacios de nombres personalizados](./developing/advanced/custom-namespaces.md)
      + [Parametrizar modelos Sling desde HTL](./developing/advanced/sling-model-parameters.md)
      + [Secretos](./developing/advanced/secrets.md)
      + [Usuarios del servicio](./developing/advanced/service-users.md)
      + [API de imagen optimizadas para la web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [Ejecutar trabajo en instancia de líder en AEM Author](./developing/advanced/run-job-on-leader-instance-in-aem-author.md)
   + Entorno de desarrollo rápido{#rde}
      + [Información general](./developing/rde/overview.md)
      + [Cómo realizar la configuración](./developing/rde/how-to-setup.md)
      + [Usos](./developing/rde/how-to-use.md)
      + [Ciclo de vida de desarrollo](./developing/rde/development-life-cycle.md)
   + Editor universal{#universal-editor}
      + Edición de la aplicación React{#react-app-editing}
         + [Información general](./developing/universal-editor/react-app/overview.md)
         + [Configuración del desarrollo local](./developing/universal-editor/react-app/local-development-setup.md)
         + [Instrumentalizar la aplicación React](./developing/universal-editor/react-app/instrument-to-edit-content.md)
   + [JavDocs de la API del SDK de AEM](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
+ Depuración de AEM{#debugging}
   + Depuración del SDK de AEM{#debugging-aem-sdk}
      + [Información general](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Registros](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Depuración remota](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Consola web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Herramientas de Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Otras herramientas](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Depuración de AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Información general](./debugging/cloud-service/overview.md)
      + [Registros](./debugging/cloud-service/logs.md)
      + [Creación e implementación](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [Explorador del repositorio](./debugging/cloud-service/repository-browser.md)
      + Riesgos{#risks}
         + [Advertencias transversales](./debugging/cloud-service/risks/traversals.md)
+ Personalización {#personalization}
   + [Información general](./personalization/overview.md)
   + [Demostración en directo](./personalization/live-demo.md)
   + Configuración{#setup}
      + [Integración con Adobe Target](./personalization/setup/integrate-adobe-target.md)
      + [Integrar etiquetas](./personalization/setup/integrate-adobe-tags.md)
   + Casos de uso {#use-cases}
      + [Experimentación (Pruebas A/B)](./personalization/use-cases/experimentation.md)
      + [Direccionamiento de comportamiento](./personalization/use-cases/behavioral-targeting.md)
      + [Personalization de usuario conocido](./personalization/use-cases/known-user-personalization.md)
+ API DE AEM{#aem-apis}
   + [Información general](./apis/overview.md)
   + OpenAPI{#openapis}
      + [Información general](./apis/openapis/overview.md)
      + [Cómo realizar la configuración](./apis/openapis/setup.md)
      + [Autenticación de servidor a servidor](./apis/openapis/use-cases/invoke-api-using-oauth-s2s.md)
      + [Autenticación de usuario (aplicación web)](./apis/openapis/use-cases/invoke-api-using-oauth-web-app.md)
      + [Autenticación de usuario (SPA)](./apis/openapis/use-cases/invoke-api-using-oauth-single-page-app.md)
      + Cómo{#how-to}
         + [Administración de perfiles de producto y credenciales](./apis/openapis/how-to/credentials-and-product-profile-management.md)
         + [Administración de permisos](./apis/openapis/how-to/services-user-group-permission-management.md)
+ Entrega de contenido{#content-delivery}
   + [Nombre de dominio personalizado](./content-delivery/custom-domain-names.md)
   + [Nombre de dominio personalizado con CDN administrado por Adobe](./content-delivery/custom-domain-name-with-adobe-managed-cdn.md)
   + [Nombre de dominio personalizado con CDN del cliente](./content-delivery/custom-domain-names-with-customer-managed-cdn.md)
   + [Almacenamiento en caché](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/caching/overview){target=_blank}
   + [CDN de Adobe - más allá del almacenamiento en caché](./content-delivery/adobe-cdn-beyond-caching.md)
   + [Páginas de error personalizadas](./content-delivery/custom-error-pages.md)
   + [Redirecciones de URL](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html?lang=es){target=_blank}
+ Almacenamiento en caché{#caching}
   + [Información general](./caching/overview.md)
   + [Servicio de AEM Publish](./caching/publish.md)
   + [Servicio de AEM Author](./caching/author.md)
   + [Análisis del índice de aciertos de caché CDN](./caching/cdn-cache-hit-ratio-analysis.md)
   + Cómo{#how-to}
      + [Habilitar el almacenamiento en caché](./caching/how-to/enable-caching.md)
      + [Deshabilitar el almacenamiento en caché](./caching/how-to/disable-caching.md)
      + [Purgar el caché](./caching/how-to/purge-cache.md)
+ Acceder a AEM{#accessing}
   + [Información general](./accessing/overview.md)
   + [Usuarios de Adobe IMS](./accessing/adobe-ims-users.md)
   + [Grupos de usuarios de IMS de Adobe](./accessing/adobe-ims-user-groups.md)
   + [Perfiles del producto IMS de Adobe](./accessing/adobe-ims-product-profiles.md)
   + [Usuarios, grupos y permisos de AEM](./accessing/aem-users-groups-and-permissions.md)
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
      + [HTTP/HTTPS para dirección IP/VPN de salida específica](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [Conexiones SQL con DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Conexiones SQL con API de SQL Java](./networking/examples/sql-java-apis.md)
      + [Servicio de correo electrónico](./networking/examples/email-service.md)
+ Seguridad {#security}
   + [Bloqueo de ataques DoS y DDoS mediante reglas de filtro de tráfico](./security/blocking-dos-attack-using-traffic-filter-rules.md)
   + Reglas de filtro de tráfico, incluidas las reglas WAF {#traffic-filter-and-waf-rules}
      + [Protección de sitios web de AEM](./security/traffic-filter-and-waf-rules/overview.md)
      + [Cómo configurar](./security/traffic-filter-and-waf-rules/setup.md)
      + [Utilizando las reglas de filtro de tráfico](./security/traffic-filter-and-waf-rules/use-cases/using-traffic-filter-rules.md)
      + [Utilizando las reglas WAF](./security/traffic-filter-and-waf-rules/use-cases/using-waf-rules.md)
      + [Prácticas recomendadas](./security/traffic-filter-and-waf-rules/best-practices.md)
      + Cómo{#how-to}
         + [Monitorización de solicitudes confidenciales](./security/traffic-filter-and-waf-rules/how-to/request-logging.md)
         + [Restricción del acceso](./security/traffic-filter-and-waf-rules/how-to/request-blocking.md)
         + [Normalización de solicitudes](./security/traffic-filter-and-waf-rules/how-to/request-transformation.md)
+ Eventos de AEM{#aem-eventing}
   + [Información general](./eventing/overview.md)
   + Ejemplos{#examples}
      + [Webhook: recibir eventos de AEM](./eventing/examples/webhook.md)
      + [Diario: cargar eventos de AEM](./eventing/examples/journaling.md)
      + [Acción de Adobe I/O Runtime: recibir eventos de AEM](./eventing/examples/runtime-action.md)
      + [Acción de Adobe I/O Runtime: procesar eventos de AEM](./eventing/examples/event-processing-using-runtime-action.md)
      + [Eventos de AEM Assets: integración de PIM](./eventing/examples/assets-pim-integration.md)
+ Migración {#migration}
   + [Herramienta de transferencia de contenido](./migration/content-transfer-tool.md)
   + [Importación masiva de recursos](./migration/bulk-import.md)
   + El paso a AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Introducción](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Incorporación](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA y CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [Herramientas de modernización de AEM](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernización del repositorio](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microservicios de Asset Compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Búsqueda e indexación](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migración de contenido {#content-migration}
         + [Servicio de importación masiva](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Herramienta de transferencia de contenido](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [Preguntas frecuentes](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [Resolución de problemas](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
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
+ [Fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html?lang=es){target=_blank}
+ Formularios{#forms}
   + Desarrollo para Forms as a Cloud Service{#developing-for-cloud-service}
      + [1 - Introducción](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - Instalar IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - Configurar Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 - Sincronizar IntelliJ con AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [&#x200B;5. Generar un formulario](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - Controlador de envío personalizado](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - Registro del servlet mediante el tipo de recurso](./forms/developing-for-cloud-service/registering-servlet-using-resourcetype.md)
      + [8 - Habilitar componentes del portal de formularios](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [9 - Incluir Cloud Services y FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
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
      + [Añadir y configurar barra de herramientas](./forms/create-first-af/add-configure-toolbar.md)
   + Servicio de envío personalizado con formulario sin encabezado{#custom-submit-headless-forms}
      + [1 - Introducción](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - Crear un servicio de envío personalizado](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - Mostrar la respuesta](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + Crear componente de bloque de direcciones{#create-address-block}
      + [1 - Introducción](./forms/create-address-block-component/introduction.md)
      + [2 - Configuración](./forms/create-address-block-component/set-up.md)
      + [3 - Crear componente](./forms/create-address-block-component/creating-address-component.md)
      + [4 - Implementar el componente](./forms/create-address-block-component/deploy-your-project.md)
   + Crear componente de imagen en el que se pueda hacer clic{#clickable-image-component}
      + [1 - Introducción](./forms/clickable-image-component/introduction.md)
      + [2 - Crear componente](./forms/clickable-image-component/create-component.md)
      + [3 - Controlar el evento de clic](./forms/clickable-image-component/handle-click-event.md)
   + AEM Forms y Analytics{#forms-and-analytics}
      + [Introducción](./forms/form-data-analytics/introduction.md)
      + [Crear elementos de datos](./forms/form-data-analytics/data-elements.md)
      + [Crear reglas](./forms/form-data-analytics/rules.md)
      + [Prueba de la solución](./forms/form-data-analytics/test.md)
   + Componente desplegable Creación de países{#countries-drop-down}
      + [Introducción](./forms/countries-drop-down/introduction.md)
      + [Crear componente](./forms/countries-drop-down/component.md)
      + [Crear cuadro de diálogo](./forms/countries-drop-down/dialog.md)
      + [Crear modelo de Sling](./forms/countries-drop-down/slingmodel.md)
      + [Generar y probar](./forms/countries-drop-down/build.md)
   + Creación de variaciones de botón{#style-system}
      + [Introducción](./forms/style-system/introduction.md)
      + [Definir política](./forms/style-system/style-policy.md)
      + [Definir variaciones](./forms/style-system/create-variations.md)
      + [Variaciones de prueba](./forms/style-system/build.md)
   + Uso de pestañas verticales{#using-vertical-tabs}
      + [&#x200B;1. Introducción](./forms/using-vertical-tabs/introduction.md)
      + [&#x200B;2. Crear formulario](./forms/using-vertical-tabs/create-af.md)
      + [&#x200B;3. Navegación](./forms/using-vertical-tabs/navigation.md)
      + [&#x200B;4. Añadir iconos](./forms/using-vertical-tabs/icons.md)
   + Usar el resultado y el servicio de formularios{#forms-cs-output-and-forms-service}
      + [Generar PDF](./forms/forms-cs-output-and-forms-service/outputservice.md)
   + Generación de documentos en AEM Forms CS{#doc-gen-formscs}
      + [Introducción](./forms/doc-gen-forms-cs/introduction.md)
      + [Crear credenciales de servicio](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Crear token JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Crear token de acceso](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Combinar datos con la plantilla](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Prueba de la solución](./forms/doc-gen-forms-cs/test.md)
      + [Desafío](./forms/doc-gen-forms-cs/challenge.md)
   + Uso de la API de servicios de documentos de Forms{#forms-document-services-api}
      + [Introducción](./forms/forms-document-services/introduction.md)
      + [Configurar OpenAPI](./forms/forms-document-services/using-open-api.md)
      + [Generar token de acceso](./forms/forms-document-services/generate-access-token.md)
      + [Aplicar derechos de uso](./forms/forms-document-services/make-api-calls.md)
      + [Código de ejemplo](./forms/forms-document-services/sample-project.md)
   + Generación de documentos mediante la API por lotes{#formscs-batch-api}
      + [Introducción](./forms/formscs-batch-api/introduction.md)
      + [Configurar Azure Storage](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Crear configuración por lotes USC](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Crear configuración por lotes](./forms/formscs-batch-api/create-batch-config.md)
      + [Ejecutar lote](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Manipulación de PDF en Forms CS{#forms-cs-assembler}
      + [Introducción](./forms/forms-cs-assembler/introduction.md)
      + [Crear credenciales de servicio](./forms/forms-cs-assembler/service-credentials.md)
      + [Crear token JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Crear token de acceso](./forms/forms-cs-assembler/create-access-token.md)
      + [Ensamblar archivos PDF](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilidades de PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Prueba de la solución](./forms/forms-cs-assembler/test.md)
      + [Desafío](./forms/forms-cs-assembler/challenge.md)
   + Almacenar envíos de formularios con etiquetas de índice de blob{#store-submiited-data-with-metadata-tags}
      + [Introducción](./forms/store-submiited-data-with-metadata-tags/introduction.md)
      + [Ampliar componente de grupo de opciones](./forms/store-submiited-data-with-metadata-tags/extend-choice-group-components.md)
      + [Crear configuración de OSGi](./forms/store-submiited-data-with-metadata-tags/create-osgi-configuration.md)
      + [Crear etiquetas de índice](./forms/store-submiited-data-with-metadata-tags/create-blob-index-tags.md)
      + [Creación de un envío personalizado](./forms/store-submiited-data-with-metadata-tags/create-custom-submit.md)
   + Rellenar previamente formulario basado en componente principal{#prefill-core-component-based-form}
      + [Introducción](./forms/prefill-core-component-form/introduction.md)
      + [Escritura del servicio de rellenado automático](./forms/prefill-core-component-form/pre-fill-service.md)
      + [Prueba de la solución](./forms/prefill-core-component-form/test-solution.md)
   + Azure Portal Storage{#forms-cs-azure-portal}
      + [Introducción](./forms/forms-cs-azure-portal/introduction.md)
      + [Crear modelo de datos de formulario](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Almacenamiento de datos de formulario en Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Rellenar previamente formulario](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Envíos de consultas](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Guardar y reanudar el rellenado de formularios{#prefill-azure-storage}
      + [1- Introducción](./forms/prefill-azure-storage/introduction.md)
      + [2 - Crear componente de página](./forms/prefill-azure-storage/page-component.md)
      + [3 - Crear plantilla de formulario adaptable](./forms/prefill-azure-storage/associate-page-component.md)
      + [4 - Crear integración de Azure Storage](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - Crear integración de SendGrid](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - Crear formulario adaptable](./forms/prefill-azure-storage/create-af.md)
      + [7 - Implementar los recursos de muestra](./forms/prefill-azure-storage/deploy-sample-assets.md)

   + Crear flujo de trabajo de revisión{#create-aem-workflow}
      + [Externalización del almacenamiento de flujo de trabajo](./forms/create-aem-workflow/externalize-workflow.md)
      + [Crear modelo de flujo de trabajo](./forms/create-aem-workflow/create-workflow.md)
      + [Activar flujo de trabajo](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign con AEM Forms{#forms-and-sign}
      + [Introducción](./forms/forms-and-sign/introduction.md)
      + [Aplicación de API de Acrobat Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuración en la nube de Acrobat Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Crear formulario adaptable](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurar para rellenar y firmar](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Integración con Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configuración de la integración](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [Analizar datos de formularios enviados](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [Enviar documento de registro como archivo adjunto de correo electrónico](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [Extraer archivos adjuntos de formularios de datos enviados](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Integración con Microsoft Dynamics{#formscs-dynamics-crm}
      + [Crear aplicación de Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configuración de fuente de datos](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Crear modelo de datos de formulario](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Crear formulario adaptable](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Integración con Salesforce{#integrate-with-salesforce}
      + [Introducción](./forms/integrate-with-salesforce/introduction.md)
      + [Crear aplicación conectada](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Crear archivo Swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [Crear fuente de datos](./forms/integrate-with-salesforce/create-data-source.md)
      + [Creación de un modelo de datos de formulario](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [Probar envío de formulario](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [Probar evento de clic](./forms/integrate-with-salesforce/create-lead-click-event.md)
   + Almacenar envíos de formularios en una unidad y SharePoint{#one-drive}
      + [Almacenar datos de formulario en una unidad](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [Almacenar datos de formulario en SharePoint](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
      + [Rellenar previamente el formulario con datos de la lista de SharePoint](./forms/forms-cs-sharepoint/prefill-data-from-sharepoint-list.md)
      + [Insertar datos en la lista de SharePoint mediante un flujo de trabajo](./forms/forms-cs-sharepoint/submit-data-sharepoint-list-workflow.md)
+ Extensibilidad de Asset Compute{#asset-compute}
   + [Información general](./asset-compute/overview.md)
   + Configurar{#set-up}
      + [Aprovisionamiento de cuentas y servicios](./asset-compute/set-up/accounts-and-services.md)
      + [Entorno de desarrollo local](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + Desarrollar{#develop}
      + [Crear un proyecto de Asset Compute](./asset-compute/develop/project.md)
      + [Configurar variables de entorno](./asset-compute/develop/environment-variables.md)
      + [Configuración de manifest.yml](./asset-compute/develop/manifest.md)
      + [Desarrollo de una herramienta](./asset-compute/develop/worker.md)
      + [Uso de la herramienta de desarrollo](./asset-compute/develop/development-tool.md)
   + Prueba y depuración{#test-debug}
      + [Probar una herramienta](./asset-compute/test-debug/test.md)
      + [Depuración de una herramienta](./asset-compute/test-debug/debug.md)
   + Implementación{#deploy}
      + [Implementación en Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Integrar con AEM](./asset-compute/deploy/processing-profiles.md)
   + Avanzado{#advanced}
      + [Trabajadores de metadatos](./asset-compute/advanced/metadata.md)
   + [Resolución de problemas](./asset-compute/troubleshooting.md)

+ Tutoriales en varios pasos{#multi-step-tutorials}
   + [Desarrollo de AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es){target=_blank}
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=es){target=_blank}
   + [Editor de SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html){target=_blank}
   + [AEM Sites y Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html?lang=es){target=_blank}
   + [Autenticación basada en token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=es){target=_blank}
+ Recursos de expertos {#expert-resources}
   + AEM Champions {#aem-champions}
      + [Manual de tácticas de incorporación de Cloud Manager](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Tipos de entorno de Cloud Manager](./expert-resources/aem-champions/environment-types.md)
      + [IU de Cloud Manager](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [Series de expertos de AEM](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [Introducción](./expert-resources/cloud-5/cloud5-introduction.md)
      + [Temporada 4](./expert-resources/cloud-5/cloud5-season-4.md)
      + [Temporada 3](./expert-resources/cloud-5/cloud5-season-3.md)
      + [Temporada 2](./expert-resources/cloud-5/cloud5-season-2.md)
      + [Temporada 1](./expert-resources/cloud-5/cloud5-season-1.md)
      + [AEM CDN, parte 1](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [AEM CDN, parte 2](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [Archivos de registro AEM](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [Tókenes de inicio de sesión](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Cloud Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [Migración 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Validador de Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [Búsqueda e indexación](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + Temporada 2{#season-2}
         + [Fragmentos](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Modernizador de repositorios](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Planificador de trabajos Sling](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [Reparar la caché](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [Corregir las reescrituras](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager: auditoría de experiencias](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager: pruebas unitarias](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager: pruebas funcionales](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
      + Temporada 3{#season-3}
         + [Búsqueda de terceros](./expert-resources/cloud-5/season-3/cloud5-3rd-party-search.md)
         + [Trabajadores de Edge](./expert-resources/cloud-5/season-3/cloud5-edge-workers.md)
         + [Publicación y cancelación de publicación de eventos en Edge Delivery Services](./expert-resources/cloud-5/season-3/cloud5-publish-events.md)
         + [Índices de consulta y fórmulas de Excel](./expert-resources/cloud-5/season-3/cloud5-query-indexes.md)
         + [Traiga su propia CDN de Cloudflare](./expert-resources/cloud-5/season-3/cloud5-byo-cloudflare-cdn.md)
         + [Integración de AEM Assets](./expert-resources/cloud-5/season-3/cloud5-integrate-assets.md)
         + [IA generativa para AEM Sites](./expert-resources/cloud-5/season-3/cloud5-generative-ai-for-aem-sites.md)
         + [Explorar el Editor universal](./expert-resources/cloud-5/season-3/cloud5-exploring-universal-editor.md)
         + [Importar sitios](./expert-resources/cloud-5/season-3/cloud5-import-sites-to-edge-delivery-services.md)
         + [Uso de la Admin API](./expert-resources/cloud-5/season-3/cloud5-using-admin-api.md)
         + [Optimización de la puntuación de Lighthouse - Parte1](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part1.md)
         + [Optimización de la puntuación de Lighthouse - Parte2](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part2.md)
         + [Optimización de la puntuación de Lighthouse - Parte3](./expert-resources/cloud-5/season-3/cloud5-lighthouse-score-optimization-part3.md)
      + Temporada 4{#season-4}
         + [Prácticas recomendadas](./expert-resources/cloud-5/season-4/cloud5-edge-delivery-services-best-practices.md)
         + [Búsqueda de optimizaciones](./expert-resources/cloud-5/season-4/cloud5-search-optimization.md)
         + [Google Maps](./expert-resources/cloud-5/season-4/cloud5-google-maps.md)
