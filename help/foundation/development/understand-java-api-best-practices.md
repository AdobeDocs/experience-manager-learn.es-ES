---
title: Java&trade; Prácticas recomendadas de API en AEM
description: AEM se basa en una pila de software de código abierto enriquecida que expone muchas API de Java&trade; para utilizarlas durante el desarrollo. Este artículo explora las principales API y cuándo y por qué deben utilizarse.
version: Experience Manager 6.4, Experience Manager 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
doc-type: Article
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
last-substantial-update: 2022-06-24T00:00:00Z
thumbnail: aem-java-bp.jpg
duration: 416
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 0%

---

# Prácticas recomendadas de API de Java™

Adobe Experience Manager (AEM) se basa en una pila de software de código abierto enriquecida que expone muchas API de Java™ para utilizarlas durante el desarrollo. Este artículo explora las principales API y cuándo y por qué deben utilizarse.

AEM se basa en cuatro conjuntos principales de API de Java™.

* **Adobe Experience Manager (AEM)**

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

Si AEM proporciona una API, preferirla sobre [!DNL Sling], JCR y OSGi. Si AEM no proporciona una API, prefiera [!DNL Sling] sobre JCR y OSGi.

Este orden es una regla general, lo que significa que existen excepciones. Los motivos aceptables para romper esta regla son:

* Excepciones conocidas, como se describe a continuación.
* La funcionalidad requerida no está disponible en una API de nivel superior.
* Operar en el contexto del código existente (código de producto personalizado o AEM) que utiliza una API menos preferida y el coste de pasar a la nueva API es injustificable.

   * Es mejor utilizar de forma consistente la API de nivel inferior que crear una mezcla.

## API DE AEM

* [**Documentos JavaScript de la API de AEM**](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/index.html)

Las API de AEM proporcionan abstracciones y funcionalidades específicas para casos de uso producidos.

Por ejemplo, las API [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) y [Page](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) de AEM proporcionan abstracciones para `cq:Page` nodos en AEM que representan páginas web.

Aunque estos nodos están disponibles a través de las API [!DNL Sling] como recursos y las API JCR como nodos, las API de AEM proporcionan abstracciones para casos de uso comunes. El uso de las API de AEM garantiza un comportamiento coherente entre AEM y el producto, así como las personalizaciones y extensiones de AEM.

### com.adobe.&#42; frente a com.day.API de &#42;

Las API de AEM tienen una preferencia dentro del paquete, identificada por los siguientes paquetes Java™, en orden de preferencia:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

El paquete `com.adobe.cq` admite casos de uso de productos, mientras que `com.adobe.granite` admite casos de uso de plataformas entre productos, como flujos de trabajo o tareas (que se utilizan en distintos productos: AEM Assets, sitios, etc.).

El paquete `com.day.cq` contiene las API &quot;originales&quot;. Estas API tratan las abstracciones y funcionalidades básicas que existían antes o alrededor de la adquisición de [!DNL Day CQ] por Adobe. Estas API son compatibles y deben evitarse, a menos que los paquetes `com.adobe.cq` o `com.adobe.granite` NO proporcionen una alternativa (más reciente).

Las nuevas abstracciones como [!DNL Content Fragments] y [!DNL Experience Fragments] se crean en el espacio `com.adobe.cq` en lugar de `com.day.cq`, como se describe a continuación.

### API de consulta

AEM admite varios idiomas de consulta. Los tres idiomas principales son [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath y [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html).

La preocupación más importante es mantener un lenguaje de consulta coherente en toda la base de código para reducir la complejidad y el coste de comprensión.

Todos los lenguajes de consulta tienen efectivamente los mismos perfiles de rendimiento, ya que [!DNL Apache Oak] los transpila en JCR-SQL2 para la ejecución final de la consulta, y el tiempo de conversión a JCR-SQL2 es insignificante en comparación con el propio tiempo de consulta.

La API preferida es [AEM Query Builder](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html), que es la abstracción de nivel más alto y proporciona una API sólida para construir, ejecutar y recuperar resultados para consultas, además de lo siguiente:

* Construcción de consultas sencilla y parametrizada (parámetros de consulta modelados como un mapa)
* [API Java™ y API HTTP nativas](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=es)
* [AEM Query Debugger](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)
* [predicados de AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/platform/query-builder/querybuilder-predicate-reference.html) que admiten requisitos de consulta comunes

* API extensible, que permite el desarrollo de [predicados de consulta personalizados](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=es)
* JCR-SQL2 y XPath se pueden ejecutar directamente a través de [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) y [API de JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), lo que devuelve los resultados de [[!DNL Sling] Resources](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) o [JCR Nodes](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), respectivamente.

>[!CAUTION]
>
>La API de AEM QueryBuilder filtra un objeto ResourceResolver. Para mitigar esta fuga, siga este [ejemplo de código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).
>

## API de [!DNL Sling]

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) es el marco web RESTful en el que se basa AEM. [!DNL Sling] proporciona enrutamiento de solicitud HTTP, modela nodos JCR como recursos, proporciona contexto de seguridad y mucho más.

Las API de [!DNL Sling] tienen la ventaja añadida de crearse para la extensión, lo que significa que a menudo es más fácil y seguro aumentar el comportamiento de las aplicaciones creadas con las API de [!DNL Sling] que las API de JCR menos ampliables.

### Usos comunes de las API [!DNL Sling]

* Acceder a nodos JCR como [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) y acceder a sus datos a través de [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Proporcionando contexto de seguridad mediante [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Creando y eliminando recursos mediante los [métodos create/move/copy/delete](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html) de ResourceResolver.
* Actualizando propiedades mediante [ModillableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Bloques de creación del procesamiento de solicitudes

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtros Servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Bloques de creación de procesamiento de trabajo asincrónico

   * [Controladores de eventos y trabajos](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Programador](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelos de Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Usuarios de servicio](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)

## API de JCR

* **[JavaDocs JCR 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Las API [JCR (repositorio de contenido Java™) 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) forman parte de una especificación para implementaciones JCR (en el caso de AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/)). Toda implementación de JCR debe cumplir e implementar estas API y, por lo tanto, es la API de nivel más bajo para interactuar con el contenido de AEM.

El JCR es un almacén de datos NoSQL basado en árbol/jerárquico que AEM utiliza como repositorio de contenido. El JCR tiene una amplia gama de API compatibles, que van desde el CRUD de contenido a la consulta de contenido. A pesar de esta API sólida, es poco frecuente que se prefieran a las abstracciones de nivel superior de AEM y [!DNL Sling].

Prefiera siempre las API de JCR sobre las API de Apache Jackrabbit Oak. Las API de JCR son para ***interactuar*** con un repositorio JCR, mientras que las API de Oak son para ***implementar*** un repositorio JCR.

### Conceptos erróneos comunes sobre las API de JCR

Aunque el JCR es el repositorio de contenido de AEM, sus API NO son el método preferido para interactuar con el contenido. En su lugar, prefiera las API de AEM (Página, Assets, Etiqueta, etc.) o las API de recursos de Sling, ya que proporcionan mejores abstracciones.

>[!CAUTION]
>
>El uso generalizado de las interfaces de sesión y nodo de las API de JCR en una aplicación de AEM huele a código. Asegúrese de que se utilicen las API [!DNL Sling] en su lugar.

### Usos comunes de las API de JCR

* [Administración de control de acceso](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Administración autorizable (usuarios/grupos)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observación de JCR (escucha de eventos JCR)
* Creación de estructuras de nodos profundos

   * Aunque las API de Sling admiten la creación de recursos, las API de JCR tienen métodos prácticos en [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) y [JcrUtil](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/commons/jcr/JcrUtil.html) que aceleran la creación de estructuras profundas.

## API de OSGi

* [**JavaDocs de OSGi R6**](https://docs.osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[JavaDocs de anotaciones de componente de OSGi Declarative Services 1.2](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Anotaciones de tipo de metal JavaDocs](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**JavaDocs de marco OSGi**](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Hay poca superposición entre las API de OSGi y las API de nivel superior (AEM, [!DNL Sling] y JCR), y la necesidad de utilizar las API de OSGi es poco frecuente y requiere un alto nivel de experiencia en desarrollo de AEM.

### API de OSGi y Apache Felix

OSGi define una especificación que todos los contenedores OSGi deben implementar y cumplir. La implementación OSGi de AEM, Apache Felix, también proporciona varias de sus propias API.

* Prefiera las API OSGi (`org.osgi`) sobre las API Apache Felix (`org.apache.felix`).

### Usos comunes de las API de OSGi

* Anotaciones OSGi para declarar servicios y componentes OSGi.

   * Prefiera [OSGi Declarative Services (DS) 1.2 Annotations](https://docs.osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) sobre [Felix SCR Annotations](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) para declarar servicios y componentes OSGi

* API OSGi para [anular el registro de servicios o componentes OSGi](https://docs.osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html) de forma dinámica en el código.

   * Prefiera el uso de anotaciones OSGi DS 1.2 cuando no sea necesario administrar el servicio o el componente OSGi condicional (como suele ser el caso).

## Excepciones a la regla

Las siguientes son excepciones comunes a las reglas definidas anteriormente.

### API de OSGi

Cuando se tratan abstracciones OSGi de bajo nivel, como definir o leer en propiedades de componentes OSGi, las abstracciones más nuevas proporcionadas por `org.osgi` se prefieren a las abstracciones de Sling de alto nivel. Las abstracciones de Sling que compiten no se han marcado como `@Deprecated` y sugieren la alternativa `org.osgi`.

Tenga en cuenta también que la definición del nodo de configuración OSGi prefiere `cfg.json` sobre el formato `sling:OsgiConfig`.

### API de recursos de AEM

* Preferir [`com.day.cq.dam.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/day/cq/dam/api/package-summary.html) sobre [`com.adobe.granite.asset.api`](https://developer.adobe.com/experience-manager/reference-materials/6-5/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Mientras que las API de Assets `com.day.cq` proporcionan herramientas más gratuitas para los casos de uso de administración de recursos de AEM.
   * Las API de Granite Assets admiten casos de uso de administración de recursos de bajo nivel (versión, relaciones).

### API de consulta

* AEM QueryBuilder no admite determinadas funciones de consulta, como [sugerencias](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), revisión ortográfica y sugerencias de índice, entre otras funciones menos comunes. Para consultar con estas funciones, se prefiere JCR-SQL2.

### Registro de servlet [!DNL Sling] {#sling-servlet-registration}

* Registro de servlet [!DNL Sling], prefiere [anotaciones OSGi DS 1.2 con @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) sobre `@SlingServlet`

### Registro de filtro [!DNL Sling] {#sling-filter-registration}

* Registro de filtro [!DNL Sling], prefiere [anotaciones OSGi DS 1.2 con @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) sobre `@SlingFilter`

## Fragmentos de código útiles

Los siguientes son fragmentos de código Java™ útiles que ilustran las prácticas recomendadas para casos de uso comunes mediante las API analizadas. Estos fragmentos también ilustran cómo pasar de API menos preferidas a API más preferidas.

### Sesión JCR a ResourceResolver [!DNL Sling]

#### Cierre automático de ResourceResolver de Sling

Desde AEM 6.2, el ResourceResolver [!DNL Sling] es `AutoClosable` en una instrucción [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). Con esta sintaxis, no es necesaria una llamada explícita a `resourceResolver .close()`.

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

ResourceResolvers se puede cerrar manualmente en un bloque `finally` si no se puede utilizar la técnica de cierre automático que se muestra arriba.

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

### [!DNL Sling] [!DNL Resource] a AEM Asset

#### Enfoque recomendado

La función `DamUtil.resolveToAsset(..)` resuelve cualquier recurso bajo `dam:Asset` en el objeto Asset subiendo por el árbol según sea necesario.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Enfoque alternativo

La adaptación de un recurso a un recurso requiere que el recurso en sí sea el nodo `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] recurso a página de AEM

#### Enfoque recomendado

`pageManager.getContainingPage(..)` resuelve cualquier recurso bajo `cq:Page` en el objeto Página subiendo por el árbol según sea necesario.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Enfoque alternativo {#alternative-approach-1}

Para adaptar un recurso a una página, el propio recurso debe ser el nodo `cq:Page`.

```java
Page page = resource.adaptTo(Page.class);
```

### Leer propiedades de página de AEM

Utilice los captadores del objeto Page para obtener propiedades conocidas (`getTitle()`, `getDescription()`, etc.) y `page.getProperties()` para obtener el ValueMap `[cq:Page]/jcr:content` para recuperar otras propiedades.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Leer propiedades de metadatos de recursos AEM

La API de recursos proporciona métodos prácticos para leer propiedades del nodo `[dam:Asset]/jcr:content/metadata`. No es un ValueMap; no se admite el segundo parámetro (valor predeterminado y conversión de tipo automático).

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Leer propiedades de [!DNL Sling] [!DNL Resource] {#read-sling-resource-properties}

Cuando las propiedades se almacenan en ubicaciones (propiedades o recursos relativos) a las que las API de AEM (Página, Recurso) no pueden acceder directamente, se pueden utilizar los recursos [!DNL Sling] y ValueMaps para obtener los datos.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

En este caso, es posible que el objeto AEM tenga que convertirse en un [!DNL Sling] [!DNL Resource] para localizar de forma eficaz la propiedad o el subrecurso deseado.

#### Página de AEM a [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### Recurso de AEM a [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Escribir propiedades utilizando ModillableValueMap de [!DNL Sling]

Use [ModillableValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) de [!DNL Sling] para escribir propiedades en nodos. Esto solo puede escribir en el nodo inmediato (no se admiten las rutas de propiedad relativas).

Tenga en cuenta que la llamada a `.adaptTo(ModifiableValueMap.class)` requiere permisos de escritura en el recurso; de lo contrario, devuelve un valor nulo.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Crear una página de AEM

Utilice siempre PageManager para crear páginas a medida que toma una Plantilla de página, es necesario para definir e inicializar correctamente las páginas en AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Crear un recurso de [!DNL Sling]

ResourceResolver admite operaciones básicas para crear recursos. Al crear abstracciones de nivel superior (páginas AEM, Assets, etiquetas, etc.), utilice los métodos proporcionados por sus respectivos responsables.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Eliminar un recurso de [!DNL Sling]

ResourceResolver admite la eliminación de un recurso. Al crear abstracciones de nivel superior (páginas AEM, Assets, etiquetas, etc.), utilice los métodos proporcionados por sus respectivos responsables.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
