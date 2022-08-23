---
title: Prácticas recomendadas de API de Java en AEM
description: AEM se basa en una pila de software de código abierto enriquecida que expone muchas API de Java para su uso durante el desarrollo. Este artículo explora las API principales y cuándo y por qué deben usarse.
version: 6.4, 6.5
feature: APIs
topic: Development
role: Developer
level: Beginner
exl-id: b613aa65-f64b-4851-a2af-52e28271ce88
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '2071'
ht-degree: 2%

---

# Prácticas recomendadas de API de Java

Adobe Experience Manager (AEM) se basa en una rica pila de software de código abierto que expone muchas API de Java para su uso durante el desarrollo. Este artículo explora las API principales y cuándo y por qué deben usarse.

AEM se basa en 4 conjuntos principales de API de Java.

* **Adobe Experience Manager (AEM)**

   * abstracciones de productos, como páginas, recursos, flujos de trabajo, etc.

* **Apache Sling Web Framework**

   * REST y abstracciones basadas en recursos, como recursos, mapas de valores y solicitudes HTTP.

* **JCR (Apache Jackrabbit Oak)**

   * Abstracciones de datos y contenido, como nodos, propiedades y sesiones.

* **OSGi (Apache Felix)**

   * abstracciones de contenedores de aplicaciones OSGi como servicios y componentes (OSGi).

## Preferencia de la API de Java &quot;regla general&quot;

La regla general es preferir las API/abstracciones en el siguiente orden:

1. **AEM**
1. **Sling**
1. **JCR**
1. **los paquetes**

Si AEM proporciona una API, preferirla [!DNL Sling], JCR y OSGi. Si AEM no proporciona una API, prefiera [!DNL Sling] sobre JCR y OSGi.

Este orden es una regla general, lo que significa que existen excepciones. Los motivos aceptables para romper con esta regla son:

* Excepciones bien conocidas, como se describe a continuación.
* La funcionalidad requerida no está disponible en una API de nivel superior.
* Operar en el contexto de código existente (código de producto personalizado o AEM) que en sí mismo utiliza una API menos preferida, y el coste de pasar a la nueva API es injustificable.

   * Es mejor utilizar la API de nivel inferior de forma consistente que crear una mezcla.

## API de AEM

* [**API de AEM JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

AEM API proporcionan abstracciones y funciones específicas de los casos de uso productizados.

Por ejemplo, AEM [PageManager](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/PageManager.html) y [Página](https://developer.adobe.com/experience-manager/reference-materials/cloud-service/javadoc/com/day/cq/wcm/api/Page.html) Las API proporcionan abstracciones para `cq:Page` nodos de AEM que representan páginas web.

Aunque estos nodos están disponibles mediante [!DNL Sling] Las API como recursos y las API de JCR como nodos, AEM API proporcionan abstracciones para casos de uso comunes. El uso de las API de AEM garantiza un comportamiento coherente entre AEM producto y las personalizaciones y extensiones de AEM.

### com.adobe.&#42; frente a com.day.&#42; API

AEM API tienen una preferencia dentro del paquete, identificada por los siguientes paquetes Java, en orden de preferencia:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` admite casos de uso de productos cuando `com.adobe.granite` admite casos de uso en plataformas de varios productos, como flujos de trabajo o tareas (que se utilizan en todos los productos: AEM Assets, Sitios, etc.).

`com.day.cq` contiene API &quot;originales&quot;. Estas API tratan de abstracciones y funcionalidades básicas que existían antes o alrededor de la adquisición de Adobe de [!DNL Day CQ]. Estas API son compatibles y no deben evitarse, a menos que `com.adobe.cq` o `com.adobe.granite` proporcione una alternativa (más reciente).

Nuevas abstracciones como [!DNL Content Fragments] y [!DNL Experience Fragments] están incorporadas en la variable `com.adobe.cq` espacio en lugar de `com.day.cq` a continuación.

### API de consulta

AEM admite varios idiomas de consulta. Los tres idiomas principales son [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath y [AEM Query Builder](https://helpx.adobe.com/es/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

La preocupación más importante es mantener un lenguaje de consulta consistente en toda la base de código, para reducir la complejidad y el coste de comprensión.

Todos los idiomas de consulta tienen efectivamente los mismos perfiles de rendimiento que [!DNL Apache Oak] los transfiere a JCR-SQL2 para la ejecución de la consulta final, y el tiempo de conversión a JCR-SQL2 es insignificante comparado con el tiempo de consulta en sí.

La API preferida es [AEM Query Builder](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), que es la abstracción de nivel más alto y proporciona una API sólida para construir, ejecutar y recuperar resultados para consultas, y proporciona lo siguiente:

* Construcción de consultas simple y parametrizada (parámetros de consulta modelados como mapa)
* Nativo [API de Java y API de HTTP](https://helpx.adobe.com/es/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [AEM Query Debugger](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [AEM predicados](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) compatibilidad con requisitos de consulta comunes

* API extensible, lo que permite el desarrollo de [predicados de consultas](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 y XPath se pueden ejecutar directamente a través de [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) y [API de JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html), devuelve los resultados a [[!DNL Sling] Recursos](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) o [Nodos JCR](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/javax/jcr/Node.html), respectivamente.

>[!CAUTION]
>
>AEM API de QueryBuilder filtra un objeto ResourceResolver. Para mitigar esta fuga, siga este [ejemplo de código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).

## [!DNL Sling] API

* [**Apache [!DNL Sling] API JavaDocs**](https://sling.apache.org/apidocs/sling10/)

[Apache [!DNL Sling]](https://sling.apache.org/) es el marco web de RESTful que AEM sustenta. [!DNL Sling] proporciona enrutamiento de solicitud HTTP, modela nodos JCR como recursos, proporciona contexto de seguridad y mucho más.

[!DNL Sling] Las API tienen la ventaja añadida de estar creadas para la extensión, lo que significa que a menudo es más fácil y seguro aumentar el comportamiento de las aplicaciones creadas mediante [!DNL Sling] API que las API de JCR menos ampliables.

### Usos comunes de [!DNL Sling] API

* Acceso a nodos JCR como [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) y acceder a sus datos mediante [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Proporcionar contexto de seguridad a través de la variable [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Creación y eliminación de recursos mediante el [métodos create/move/copy/delete](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Actualización de propiedades mediante la variable [MapaValorModificable](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Componentes básicos del procesamiento de solicitudes de creación

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtros Servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Componentes básicos del procesamiento de trabajo asincrónico

   * [Gestores de eventos y trabajos](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planificador](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelos Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Usuarios de servicio](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## API de JCR

* **[JavaDocs de JCR 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

La variable [API de JCR (repositorio de contenido Java) 2.0](https://developer.adobe.com/experience-manager/reference-materials/spec/javax.jcr/javadocs/jcr-2.0/index.html) forma parte de una especificación para implementaciones JCR (en el caso de AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Toda la implementación de JCR debe cumplir e implementar estas API y, por lo tanto, es la API de nivel más bajo para interactuar con AEM contenido.

El propio JCR es un almacén de datos NoSQL basado en árboles y jerárquico que AEM utiliza como repositorio de contenido. El JCR tiene una amplia gama de API compatibles, que van desde el CRUD de contenido hasta la consulta de contenido. A pesar de esta API robusta, es raro que sean preferidos sobre los AEM de nivel superior y [!DNL Sling] abstracciones.

Siempre prefiera las API de JCR en lugar de las API de Apache Jackrabbit Oak. Las API de JCR están destinadas a ***interacción*** con un repositorio JCR, mientras que las API de Oak son para ***implementación*** un repositorio JCR.

### Conceptos erróneos comunes sobre las API de JCR

Aunque el JCR es AEM repositorio de contenido, sus API NO son el método preferido para interactuar con el contenido. En su lugar, prefiera las API de AEM (página, recursos, etiqueta, etc.) o las API de recursos de Sling, ya que proporcionan mejores abstracciones.

>[!CAUTION]
>
>El uso generalizado de las interfaces de nodo y sesión de las API de JCR en una aplicación AEM es el olor del código. Asegúrese [!DNL Sling] Las API no deben usarse en su lugar.

### Uso común de las API de JCR

* [Gestión del control de acceso](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html)
* [Administración autorizada (usuarios/grupos)](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observación JCR (escucha de eventos JCR)
* Creación de estructuras de nodos profundos

   * Aunque las API de Sling admiten la creación de recursos, las API de JCR tienen métodos de conveniencia en [JcrUtils](https://jackrabbit.apache.org/api/2.12/org/apache/jackrabbit/commons/JcrUtils.html) y [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) que aceleran la creación de estructuras profundas.

## API de OSGi

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2 Anotaciones de componentes JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Anotaciones de tipo de metadatos JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Hay poca superposición entre las API de OSGi y las API de nivel superior (AEM, [!DNL Sling], y JCR), y la necesidad de utilizar las API de OSGi es poco frecuente y requiere un alto nivel de experiencia en desarrollo de AEM.

### OSGi frente a las API de Apache Felix

OSGi define una especificación a la que todos los contenedores OSGi deben implementarse y ajustarse. AEM implementación OSGi, Apache Felix, también proporciona varias de sus propias API.

* Preferir las API de OSGi (`org.osgi`) sobre las API de Apache Felix (`org.apache.felix`).

### Uso común de las API de OSGi

* Anotaciones OSGi para declarar servicios y componentes OSGi.

   * Preferir [OSGi Declarative Services (DS) 1.2 Anotaciones](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) over [Anotaciones Félix SCR](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) para declarar servicios y componentes OSGi

* API de OSGi para código de forma dinámica [cancelación/registro de servicios/componentes OSGi](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html).

   * Prefiera el uso de anotaciones OSGi DS 1.2 cuando no se necesite la administración condicional de OSGi Service/Component (que suele ser el caso).

## Excepciones a la regla

Las siguientes son excepciones comunes a las reglas definidas anteriormente.

### API de OSGi

Cuando se trata de abstracciones OSGi de bajo nivel, como definir o leer en propiedades de componentes OSGi, las abstracciones más recientes proporcionadas por `org.osgi` son preferibles a las absacciones de Sling de nivel superior. Las abstracciones de Sling de la competencia no se han marcado como `@Deprecated` y sugieran `org.osgi` alternativa.

Tenga en cuenta también que la definición del nodo de configuración OSGi prefiere `cfg.json` sobre el `sling:OsgiConfig` formato.

### API de AEM Asset

* Preferir [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) over [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Mientras que la variable `com.day.cq` Las API de recursos proporcionan herramientas más gratuitas para casos prácticos AEM administración de recursos.
   * Las API de Granite Assets admiten casos de uso de administración de recursos de bajo nivel (versión, relaciones).

### API de consulta

* AEM QueryBuilder no admite determinadas funciones de consulta, como [sugerencias](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), la revisión ortográfica y las sugerencias de índice, entre otras funciones menos comunes. Para consultar con estas funciones se prefiere JCR-SQL2.

### [!DNL Sling] Registro de servlet {#sling-servlet-registration}

* [!DNL Sling] registro de servlet, preferir [Anotaciones OSGi DS 1.2 con @SlingServletResourceTypes](https://sling.apache.org/documentation/the-sling-engine/servlets.html) over `@SlingServlet`

### [!DNL Sling] Registro de filtros {#sling-filter-registration}

* [!DNL Sling] filtro de registro, preferir [Anotaciones OSGi DS 1.2 con @SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) over `@SlingFilter`

## Fragmentos de código útiles

Los siguientes son fragmentos útiles de código Java que ilustran las prácticas recomendadas para casos de uso comunes usando API discutidas. Estos fragmentos también ilustran cómo pasar de las API preferidas a las más preferidas.

### Sesión de JCR a [!DNL Sling] ResourceResolver

#### Cerrar automáticamente ResourceResolver de Sling

Desde el AEM 6.2, la variable [!DNL Sling] ResourceResolver es `AutoClosable` en un [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) instrucción. Con esta sintaxis, una llamada explícita a `resourceResolver .close()` no es necesario.

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

#### ResourceResolver de Sling cerrado manualmente

ResourceResolvers se puede cerrar manualmente en un `finally` , si no puede utilizarse la técnica de cierre automático que se muestra arriba.

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

### Ruta de JCR a [!DNL Sling] [!DNL Resource]

```java
Resource resource = ResourceResolver.getResource("/path/to/the/resource");
```

### Nodo JCR a [!DNL Sling] [!DNL Resource]

```java
Resource resource = resourceResolver.getResource(node.getPath());
```

### [!DNL Sling] [!DNL Resource] a AEM recurso

#### Enfoque recomendado

`DamUtil.resolveToAsset(..)`resuelve cualquier recurso en el `dam:Asset` para acceder al objeto Asset , suba por el árbol según sea necesario.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Enfoque alternativo

La adaptación de un recurso a un recurso requiere que el recurso en sí sea la variable `dam:Asset` nodo .

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Recurso a AEM página

#### Enfoque recomendado

`pageManager.getContainingPage(..)` resuelve cualquier recurso en el `cq:Page` para acceder al objeto Página , suba por el árbol según sea necesario.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Enfoque alternativo {#alternative-approach-1}

La adaptación de un recurso a una página requiere que el propio recurso sea la `cq:Page` nodo .

```java
Page page = resource.adaptTo(Page.class);
```

### Leer AEM propiedades de página

Utilice los captadores del objeto Página para obtener propiedades conocidas (`getTitle()`, `getDescription()`, etc.) y `page.getProperties()` para obtener el `[cq:Page]/jcr:content` ValueMap para recuperar otras propiedades.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Leer AEM propiedades de metadatos de recursos

La API de recursos proporciona métodos prácticos para leer propiedades desde el `[dam:Asset]/jcr:content/metadata` nodo . Tenga en cuenta que no se trata de un ValueMap, no se admite el segundo parámetro (valor predeterminado y conversión de tipo automático).

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Lectura [!DNL Sling] [!DNL Resource] propiedades {#read-sling-resource-properties}

Cuando las propiedades se almacenan en ubicaciones (propiedades o recursos relativos) en las que las API de AEM (página, recurso) no pueden acceder directamente, la variable [!DNL Sling] Se pueden utilizar Resources y ValueMaps para obtener los datos.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

En este caso, es posible que el objeto AEM tenga que convertirse en un [!DNL Sling] [!DNL Resource] para localizar de forma eficaz la propiedad o el subrecurso que desee.

#### AEM Página a [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM recurso a [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Escribir propiedades mediante [!DNL Sling]s ModisibleValueMap

Uso [!DNL Sling]&#39;s [MapaValorModificable](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) para escribir propiedades en nodos. Esto solo puede escribir en el nodo inmediato (no se admiten las rutas de propiedad relativas).

Tenga en cuenta la llamada a `.adaptTo(ModifiableValueMap.class)` requiere permisos de escritura para el recurso; de lo contrario, devolverá un valor nulo.

```java
ModifiableValueMap properties = resource.adaptTo(ModifiableValueMap.class);

properties.put("newPropertyName", "new value");
properties.put("propertyNameToUpdate", "updated value");
properties.remove("propertyToRemove");

resource.getResourceResolver().commit();
```

### Crear una página AEM

Utilice siempre PageManager para crear páginas, ya que se necesita una plantilla de página, para definir e inicializar correctamente las páginas en AEM.

```java
String templatePath = "/conf/my-app/settings/wcm/templates/content-page";
boolean autoSave = true;

PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
pageManager.create("/content/parent/path", "my-new-page", templatePath, "My New Page Title", autoSave);

if (!autoSave) { resourceResolver.commit(); }
```

### Cree un [!DNL Sling] Recurso

ResourceResolver admite operaciones básicas para crear recursos. Al crear abstracciones de nivel superior (AEM páginas, recursos, etiquetas, etc.) utilice los métodos proporcionados por sus respectivos administradores.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Eliminar un [!DNL Sling] Recurso

ResourceResolver admite la eliminación de un recurso. Al crear abstracciones de nivel superior (AEM páginas, recursos, etiquetas, etc.) utilice los métodos proporcionados por sus respectivos administradores.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
