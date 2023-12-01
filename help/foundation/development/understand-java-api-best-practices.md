---
title: AEM Java&trade; Prácticas recomendadas de API en el sector de la
description: AEM La se basa en una pila de software de código abierto enriquecida que expone muchas API de Java&trade; para usarlas durante el desarrollo. Este artículo explora las principales API y cuándo y por qué deben utilizarse.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2079'
ht-degree: 2%

---

# Prácticas recomendadas de API de Java™

Adobe Experience Manager AEM () se basa en una pila de software de código abierto enriquecida que expone muchas API de Java™ para utilizarlas durante el desarrollo. Este artículo explora las principales API y cuándo y por qué deben utilizarse.

AEM Se basa en cuatro conjuntos principales de API de Java™.

* **Adobe Experience Manager AEM ()**

   * Abstracciones de productos como páginas, recursos, flujos de trabajo, etc.

* **Marco web de Apache Sling**

   * REST y abstracciones basadas en recursos como recursos, mapas de valores y solicitudes HTTP.

* **JCR (Apache Jackrabbit Oak)**

   * Resumen de datos y contenido, como nodos, propiedades y sesiones.

* **OSGi (Apache Felix)**

   * Abstracciones del contenedor de aplicaciones OSGi, como servicios y componentes (OSGi).

## Preferencia de la API de Java™ &quot;regla general&quot;

La regla general es preferir las API/abstracciones en el siguiente orden:

1. **AEM**
1. **Sling**
1. **JCR**
1. **OSGi**

AEM Si proporciona una API, prefiera esta opción en lugar de [!DNL Sling], JCR y OSGi. AEM Si no proporciona una API, prefiera lo siguiente: [!DNL Sling] sobre JCR y OSGi.

Este orden es una regla general, lo que significa que existen excepciones. Los motivos aceptables para romper esta regla son:

* Excepciones conocidas, como se describe a continuación.
* La funcionalidad requerida no está disponible en una API de nivel superior.
* AEM Operar en el contexto de un código existente (código de producto personalizado o) que utiliza una API menos preferida y el coste de pasar a la nueva API es injustificable.

   * Es mejor utilizar de forma consistente la API de nivel inferior que crear una mezcla.

## AEM API de

* [**AEM JavaDocs de API de**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

AEM Las API de proporcionan abstracciones y funcionalidades específicas para casos de uso producidos.

AEM Por ejemplo, la opción de [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) y [Página](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) Las API ofrecen abstracciones para `cq:Page` AEM nodos en las páginas web que representan páginas web.

Mientras que estos nodos están disponibles mediante [!DNL Sling] AEM API como recursos y API de JCR como nodos, las API de proporcionan abstracciones para casos de uso comunes. AEM AEM AEM El uso de las API de garantiza un comportamiento coherente entre el producto, las personalizaciones y las extensiones de los productos y las extensiones de los que se puede obtener un.

### com.adobe.&#42; frente a com.day.&#42; API

AEM Las API de tienen una preferencia dentro del paquete, identificada por los siguientes paquetes Java™, en orden de preferencia:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

El `com.adobe.cq` El paquete admite casos de uso del producto, mientras que `com.adobe.granite` admite casos de uso de plataformas entre productos, como flujos de trabajo o tareas (que se utilizan en distintos productos: AEM Assets, Sites, etc.).

El `com.day.cq` El paquete contiene las API &quot;originales&quot;. Estas API tratan sobre las abstracciones y funcionalidades básicas que existían antes o en torno a la adquisición de Adobe de [!DNL Day CQ]. Estas API son compatibles y deben evitarse, a menos que `com.adobe.cq` o `com.adobe.granite` paquetes NO proporciona una alternativa (más reciente).

Nuevas abstracciones como [!DNL Content Fragments] y [!DNL Experience Fragments] se crean en el `com.adobe.cq` espacio en lugar de `com.day.cq` se describe a continuación.

### API de consulta

AEM admite varios idiomas de consulta. Los tres idiomas principales son [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath y [AEM Generador de consultas](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

La preocupación más importante es mantener un lenguaje de consulta coherente en toda la base de código para reducir la complejidad y el coste de comprensión.

Todos los idiomas de consulta tienen efectivamente los mismos perfiles de rendimiento, como [!DNL Apache Oak] los transpila a JCR-SQL2 para la ejecución final de la consulta, y el tiempo de conversión a JCR-SQL2 es insignificante en comparación con el propio tiempo de consulta.

La API preferida es [AEM Generador de consultas](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html), que es la abstracción de nivel más alto y proporciona una API sólida para construir, ejecutar y recuperar resultados para consultas, además de lo siguiente:

* Construcción de consultas sencilla y parametrizada (parámetros de consulta modelados como un mapa)
* Nativo [API de Java™ y HTTP](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=es)
* [AEM Depurador de consultas](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [AEM predicados de](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html) compatibilidad con requisitos comunes de consultas

* API extensible, que permite el desarrollo de funciones personalizadas [predicados de consulta](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=es)
* JCR-SQL2 y XPath se pueden ejecutar directamente mediante [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) y [API de JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), devolviendo resultados a [[!DNL Sling] Recursos](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) o [Nodos JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), respectivamente.

>[!CAUTION]
>
>AEM La API de QueryBuilder filtra un objeto ResourceResolver. Para mitigar esta fuga, siga este [ejemplo de código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## [!DNL Sling] API

* [**Apache [!DNL Sling] JavaDocs de API**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) AEM es el marco web RESTful en el que se basa la aplicación de la. [!DNL Sling] proporciona enrutamiento de solicitud HTTP, modela nodos JCR como recursos, proporciona contexto de seguridad y mucho más.

[!DNL Sling] Las API tienen la ventaja añadida de crearse para la extensión, lo que significa que a menudo es más fácil y seguro aumentar el comportamiento de las aplicaciones creadas con [!DNL Sling] API que las API de JCR menos ampliables.

### Usos comunes de [!DNL Sling] API

* Acceder a nodos JCR como [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) y acceso a sus datos mediante [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Proporcionar contexto de seguridad a través de [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Crear y eliminar recursos mediante el [métodos create/move/copy/delete](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Actualización de propiedades mediante la variable [ModillableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Bloques de creación del procesamiento de solicitudes

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtros Servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Bloques de creación de procesamiento de trabajo asincrónico

   * [Controladores de eventos y trabajos](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planificador](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelos de Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Usuarios de servicio](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## API de JCR

* **[JavaDocs JCR 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

El [API de JCR (repositorio de contenido Java™) 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) AEM forma parte de una especificación para implementaciones de JCR (en el caso de la implementación de un JCR, en el caso de la implementación de un JCR), [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). AEM Toda implementación de JCR debe ajustarse a estas API e implementarlas; por lo tanto, es la API de nivel más bajo para interactuar con contenido de.

AEM El propio JCR es un almacén de datos NoSQL basado en árbol/jerárquico que utiliza como repositorio de contenido el. El JCR tiene una amplia gama de API compatibles, que van desde el CRUD de contenido a la consulta de contenido. AEM A pesar de esta API robusta, es raro que se prefieran en lugar de las API de nivel superior y de nivel superior, y [!DNL Sling] abstracciones.

Prefiera siempre las API de JCR sobre las API de Apache Jackrabbit Oak. Las API de JCR son para ***interacción*** con un repositorio JCR, mientras que las API de Oak son para ***implementación*** un repositorio JCR.

### Conceptos erróneos comunes sobre las API de JCR

AEM Aunque el JCR es un repositorio de contenido en el que se realiza un seguimiento, sus API NO son el método preferido para interactuar con el contenido. AEM En su lugar, prefiera las API de recursos de Sling (Página, Recursos, Etiqueta, etc.) o las API de recursos de Sling, ya que proporcionan mejores abstracciones.

>[!CAUTION]
>
>AEM El uso generalizado de las interfaces de sesión y nodo de las API de JCR en una aplicación de huele a código. Asegurar [!DNL Sling] Deben utilizarse API en su lugar.

### Usos comunes de las API de JCR

* [Administración de control de acceso](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Administración autorizable (usuarios/grupos)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observación de JCR (escucha de eventos JCR)
* Creación de estructuras de nodos profundos

   * Aunque las API de Sling admiten la creación de recursos, las API de JCR tienen métodos prácticos en [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) y [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) que aceleran la creación de estructuras profundas.

## API de OSGi

* [**JavaDocs de OSGi R6**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs de anotaciones de componente de OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Anotaciones de tipos de metal JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs de marco OSGi**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

AEM Hay poca superposición entre las API de OSGi y las API de nivel superior (, [!DNL Sling]AEM , y JCR), y la necesidad de utilizar las API de OSGi es poco frecuente y requiere un alto nivel de experiencia en desarrollo de.

### API de OSGi y Apache Felix

OSGi define una especificación que todos los contenedores OSGi deben implementar y cumplir. AEM La implementación de OSGi, Apache Felix, también proporciona varias de sus propias API.

* Preferir las API de OSGi (`org.osgi`) sobre las API de Apache Felix (`org.apache.felix`).

### Usos comunes de las API de OSGi

* Anotaciones OSGi para declarar servicios y componentes OSGi.

   * Preferir [Anotaciones de OSGi Declarative Services (DS) 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) sobre [Anotaciones de Felix SCR](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) para declarar servicios y componentes OSGi

* API OSGi para contenido en código de forma dinámica [anular el registro de servicios o componentes OSGi](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Prefiera el uso de anotaciones OSGi DS 1.2 cuando no sea necesario administrar el servicio o el componente OSGi condicional (como suele ser el caso).

## Excepciones a la regla

Las siguientes son excepciones comunes a las reglas definidas anteriormente.

### API de OSGi

Cuando se tratan abstracciones OSGi de bajo nivel, como definir o leer en propiedades de componentes OSGi, las abstracciones más nuevas proporcionadas por `org.osgi` se prefieren a las abstracciones de Sling de nivel superior. Las abstracciones de Sling competidoras no se han marcado como `@Deprecated` y sugerir el `org.osgi` alternativa.

Tenga en cuenta también la preferencia de definición del nodo de configuración OSGi `cfg.json` sobre el `sling:OsgiConfig` formato.

### AEM API de recursos de

* Preferir [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) sobre [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Mientras que el `com.day.cq` AEM Las API de Assets proporcionan herramientas más gratuitas para los casos de uso de administración de recursos de los que se puede hacer uso.
   * Las API de Granite Assets admiten casos de uso de administración de recursos de bajo nivel (versión, relaciones).

### API de consulta

* AEM QueryBuilder no es compatible con determinadas funciones de consulta, como [sugerencias](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), revisión ortográfica y sugerencias de índice, entre otras funciones menos comunes. Para consultar con estas funciones, se prefiere JCR-SQL2.

### [!DNL Sling] Registro de servlet {#sling-servlet-registration}

* [!DNL Sling] registro de servlet, preferencia [Anotaciones de OSGi DS 1.2 con @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) sobre `@SlingServlet`

### [!DNL Sling] Filtrar registro {#sling-filter-registration}

* [!DNL Sling] filtrar registro, preferir [Anotaciones de OSGi DS 1.2 con @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) sobre `@SlingFilter`

## Fragmentos de código útiles

Los siguientes son fragmentos de código Java™ útiles que ilustran las prácticas recomendadas para casos de uso comunes mediante las API analizadas. Estos fragmentos también ilustran cómo pasar de API menos preferidas a API más preferidas.

### Sesión JCR a [!DNL Sling] ResourceResolver

#### Cierre automático de ResourceResolver de Sling

AEM Desde la versión 6.2, la [!DNL Sling] ResourceResolver es `AutoClosable` en un [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) declaración. Con esta sintaxis, se llama explícitamente a `resourceResolver .close()` no es necesario.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

try (ResourceResolver resourceResolver = rrf.getResourceResolver(authInfo)) {
    // Do work with the resourceResolver
} catch (LoginException e) { .. }
```

#### Sling ResourceResolver cerrado manualmente

ResourceResolvers se puede cerrar manualmente en un `finally` , si no se puede utilizar la técnica de cierre automático que se muestra arriba.

```java
@Reference
ResourceResolverFactory rrf;
...
Map<String, Object> authInfo = new HashMap<String, Object>();
authInfo.put(JcrResourceConstants.AUTHENTICATION_INFO_SESSION, jcrSession);

ResourceResolver resourceResolver = null;

try {
    resourceResolver = rrf.getResourceResolver(authInfo);
    // Do work with the resourceResolver
} catch (LoginException e) {
   ...
} finally {
    if (resourceResolver != null) { resourceResolver.close(); }
}
```

### Ruta JCR a [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Nodo JCR a [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] AEM a recurso de

#### Enfoque recomendado

El `DamUtil.resolveToAsset(..)` resuelve cualquier recurso en la variable `dam:Asset` Vaya al objeto Asset subiendo por el árbol según sea necesario.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Enfoque alternativo

Para adaptar un recurso a un recurso, el propio recurso debe ser `dam:Asset` nodo.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] AEM Recurso a página de la

#### Enfoque recomendado

`pageManager.getContainingPage(..)` resuelve cualquier recurso en `cq:Page` Vaya al objeto Página subiendo por el árbol según sea necesario.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Enfoque alternativo {#alternative-approach-1}

Para adaptar un recurso a una página es necesario que el propio recurso sea `cq:Page` nodo.

```java
Page page = resource.adaptTo(Page.class);
```

### AEM Leer propiedades de página

Utilice los captadores del objeto Page para obtener propiedades conocidas (`getTitle()`, `getDescription()`, etc.) y `page.getProperties()` para obtener la `[cq:Page]/jcr:content` ValueMap para recuperar otras propiedades.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### AEM Leer propiedades de metadatos de recursos

La API de recursos proporciona métodos prácticos para leer propiedades de `[dam:Asset]/jcr:content/metadata` nodo. No es un ValueMap; no se admite el segundo parámetro (valor predeterminado y conversión de tipo automático).

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Leer [!DNL Sling] [!DNL Resource] propiedades {#read-sling-resource-properties}

AEM Cuando las propiedades se almacenan en ubicaciones (propiedades o recursos relativos) a las que las API de (Página, Recurso) no pueden acceder directamente, la variable [!DNL Sling] Los recursos y ValueMaps se pueden utilizar para obtener los datos.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

AEM En este caso, es posible que el objeto de la tenga que convertirse en un [!DNL Sling] [!DNL Resource] para localizar de forma eficaz la propiedad o el subrecurso deseado.

#### AEM a [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM Recurso de a [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Escribir propiedades mediante [!DNL Sling]ModillableValueMap de

Uso [!DNL Sling]de [ModillableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) para escribir propiedades en nodos. Esto solo puede escribir en el nodo inmediato (no se admiten las rutas de propiedad relativas).

Observe la llamada a `.adaptTo(ModifiableValueMap.class)` requiere permisos de escritura en el recurso; de lo contrario, devuelve un valor nulo.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### AEM Creación de una página de

AEM Utilice siempre PageManager para crear páginas a medida que se necesita una plantilla de página; es necesario para definir e inicializar correctamente las páginas en los recursos de la plantilla de página de.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Crear un [!DNL Sling] Recurso

ResourceResolver admite operaciones básicas para crear recursos. AEM Al crear abstracciones de nivel superior (Páginas, Recursos, Etiquetas, etc.), utilice los métodos proporcionados por sus respectivos responsables.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Eliminar un [!DNL Sling] Recurso

ResourceResolver admite la eliminación de un recurso. AEM Al crear abstracciones de nivel superior (Páginas, Recursos, Etiquetas, etc.), utilice los métodos proporcionados por sus respectivos responsables.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
