---
title: Comprender las optimizaciones de la API de Java en AEM
description: AEM está basado en una rica pila de software de código abierto que expone muchas API de Java para su uso durante el desarrollo. Este artículo explora las principales API y cuándo y por qué deben utilizarse.
version: 6.2, 6.3, 6.4, 6.5
sub-product: fundaciones, recursos, sitios
feature: null
topics: best-practices, development
activity: develop
audience: developer
doc-type: article
translation-type: tm+mt
source-git-commit: fcb47ee3878f6a789b2151e283431c4806e12564
workflow-type: tm+mt
source-wordcount: '2023'
ht-degree: 1%

---


# Comprender las optimizaciones de la API de Java

Adobe Experience Manager (AEM) está basado en una rica pila de software de código abierto que expone muchas API de Java para su uso durante el desarrollo. Este artículo explora las principales API y cuándo y por qué deben utilizarse.

AEM se basa en 4 conjuntos de API de Java principales.

* **Adobe Experience Manager (AEM)**

   * abstracciones de productos como páginas, recursos, flujos de trabajo, etc.

* **[!DNL Apache Sling]Web Framework**

   * REST y abstracciones basadas en recursos como recursos, mapas de valores y solicitudes HTTP.

* **JCR ([!DNL Apache Jackrabbit Oak])**

   * abstracciones de datos y contenido como nodos, propiedades y sesiones.

* **[!DNL OSGi (Apache Felix)]**

   * abstracciones de contenedores de aplicaciones OSGi como servicios y componentes (OSGi).

## Preferencia de la API de Java &quot;regla general&quot;

La regla general es preferir las API/abstracciones en el siguiente orden:

1. **AEM**
1. **[!DNL Sling]**
1. **JCR**
1. **los paquetes**

Si AEM proporciona una API, preferirla a [!DNL Sling], JCR y OSGi. Si AEM no proporciona una API, prefiera [!DNL Sling] por encima de JCR y OSGi.

Este orden es una regla general, lo que significa que existen excepciones. Los motivos aceptables para romper con esta regla son:

* Excepciones bien conocidas, como se describe a continuación.
* La funcionalidad requerida no está disponible en una API de nivel superior.
* Operar en el contexto de código existente (código de producto personalizado o AEM) que utiliza una API menos preferida, y el costo de pasar a la nueva API es injustificable.

   * Es mejor utilizar de forma consistente la API de nivel inferior que crear una mezcla.

## API AEM

* [**AEM API JavaDocs**](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/index.html)

Las API de AEM proporcionan abstracciones y funcionalidades específicas para casos de uso productizados.

Por ejemplo, las API AEM [PageManager](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/api/PageManager.html) y [Page](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/wcm/foundation/model/Page.html) proporcionan abstracciones para nodos `cq:Page` en AEM que representan páginas Web.

Aunque estos nodos están disponibles mediante [!DNL Sling] API como recursos y API de JCR como nodos, las API de AEM proporcionan abstracciones para casos de uso comunes. El uso de las API de AEM garantiza un comportamiento coherente entre AEM el producto y las personalizaciones y extensiones de AEM.

### com.adobe.* vs. com.day.* API

Las API de AEM tienen una preferencia dentro del paquete, identificada por los siguientes paquetes de Java, en orden de preferencia:

1. `com.adobe.cq`
1. `com.adobe.granite`
1. `com.day.cq`

`com.adobe.cq` admite casos de uso de productos, mientras que  `com.adobe.granite` admite casos de uso de plataformas entre productos, como flujo de trabajo o tareas (que se utilizan en todos los productos: AEM Assets, Sitios, etc.).

`com.day.cq` contiene API &quot;originales&quot;. Estas API abordan abstracciones y funcionalidades principales que existían antes o alrededor de la adquisición por parte del Adobe de [!DNL Day CQ]. Estas API son compatibles y no deben evitarse, a menos que `com.adobe.cq` o `com.adobe.granite` proporcionen una alternativa (más reciente).

Las nuevas abstracciones como [!DNL Content Fragments] y [!DNL Experience Fragments] se crean en el espacio `com.adobe.cq` en lugar de `com.day.cq` que se describen a continuación.

### API de consulta

AEM admite varios idiomas de consulta. Los 3 idiomas principales son [JCR-SQL2](https://docs.jboss.org/jbossdna/0.7/manuals/reference/html/jcr-query-and-search.html), XPath y [Generador de Consultas de AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html).

La preocupación más importante es mantener un lenguaje de consulta consistente en toda la base de código, para reducir la complejidad y el costo de comprender.

Todos los lenguajes de consulta tienen efectivamente los mismos perfiles de rendimiento, ya que [!DNL Apache Oak] los transpila a JCR-SQL2 para la ejecución final de la consulta, y el tiempo de conversión a JCR-SQL2 es insignificante en comparación con el tiempo de consulta mismo.

La API preferida es [AEM Generador de Consultas](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html), que es la abstracción de nivel más alto y proporciona una API sólida para construir, ejecutar y recuperar resultados para consultas, y proporciona lo siguiente:

* Construcción de consultas sencilla y parametrizada (parámetros de consulta modelados como mapa)
* API nativa [Java y API HTTP](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/querybuilder-api.html)
* [Depurador de Consulta OOTB](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-api.html#TestingandDebugging)
* [Predicados ](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/querybuilder-predicate-reference.html) de OOTB que admiten requisitos de consulta comunes

* API extensible que permite el desarrollo de [predicados de consulta personalizados](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/implementing-custom-predicate-evaluator.html)
* JCR-SQL2 y XPath se pueden ejecutar directamente a través de [[!DNL Sling]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html#findResources-java.lang.String-java.lang.String-) y [API de JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/query/package-summary.html), lo que devuelve resultados de [[!DNL Sling] Recursos](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) o [Nodos de JCR](https://docs.adobe.com/docs/en/spec/jsr170/javadocs/jcr-2.0/javax/jcr/Node.html), respectivamente.

>[!CAUTION]
>
>AEM API de QueryBuilder filtra un objeto ResourceResolver. Para mitigar esta filtración, siga esta [muestra de código](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/search/querybuilder/impl/SampleQueryBuilder.java#L164).


## [!DNL Sling] API

* [**JavaDocs de  [!DNL Sling] API de Apache**](https://sling.apache.org/apidocs/sling10/)

[ [!DNL Sling]](https://sling.apache.org/) Apaca el marco web RESTful que sirve de base para AEM. [!DNL Sling] proporciona enrutamiento de solicitud HTTP, modela nodos JCR como recursos, proporciona contexto de seguridad y mucho más.

[!DNL Sling] Las API tienen la ventaja añadida de ser creadas para la extensión, lo que significa que a menudo es más fácil y seguro aumentar el comportamiento de las aplicaciones creadas con  [!DNL Sling] API que las API de JCR menos extensibles.

### Usos comunes de las API [!DNL Sling]

* Acceso a nodos JCR como [[!DNL Sling Resources]](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/Resource.html) y acceso a sus datos mediante [ValueMaps](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ValueMap.html).

* Proporcionar contexto de seguridad mediante [ResourceResolver](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html).
* Creación y eliminación de recursos mediante los [métodos create/move/copy/delete](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ResourceResolver.html) de ResourceResolver.
* Actualizando propiedades mediante [ModifiedValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html).
* Generación de bloques de creación de procesamiento de solicitudes

   * [Servlets](https://sling.apache.org/documentation/the-sling-engine/servlets.html)
   * [Filtros de servlet](https://sling.apache.org/documentation/the-sling-engine/filters.html)

* Componentes básicos del procesamiento de trabajo asincrónico

   * [Controladores de evento y trabajo](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html)
   * [Planificador](https://sling.apache.org/documentation/bundles/scheduler-service-commons-scheduler.html)
   * [Modelos Sling](https://sling.apache.org/documentation/bundles/models.html)

* [Usuarios de servicios](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)

## API de JCR

* **[JavaDocs JCR 2.0](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html)**

Las [API de JCR (Java Content Repository) 2.0](https://docs.adobe.com/docs/en/spec/javax.jcr/javadocs/jcr-2.0/index.html) forman parte de una especificación para implementaciones de JCR (en el caso de AEM, [Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/)). Toda la implementación de JCR debe cumplir e implementar estas API y, por lo tanto, es la API de nivel más bajo para interactuar con contenido AEM.

El propio JCR es un almacén de datos noSQL basado en árbol y jerárquico que AEM utiliza como repositorio de contenido. El JCR cuenta con una amplia gama de API admitidas, que van desde CRUD de contenido hasta consultas de contenido. A pesar de esta sólida API, es raro que sean preferibles a las abstracciones de alto nivel AEM y [!DNL Sling].

Siempre prefiera las API de JCR por sobre las API de Apache Jackrabbit Oak. Las API de JCR están destinadas a ***interactuar*** con un repositorio de JCR, mientras que las API de Oak están destinadas a ***implementar*** un repositorio de JCR.

### Conceptos erróneos comunes sobre las API de JCR

Aunque el JCR es AEM repositorio de contenido, sus API NO son el método preferido para interactuar con el contenido. En su lugar, prefiera las API de AEM (Página, Recursos, Etiqueta, etc.) o las API de recursos de Sling, ya que proporcionan mejores abstracciones.

>[!CAUTION]
>
>El uso amplio de las interfaces Session y Node de las API de JCR en una aplicación AEM es olor a código. Asegúrese de que las API [!DNL Sling] no deben usarse en su lugar.

### Usos comunes de las API de JCR

* [Gestión de controles de acceso](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html)
* [Administración autorizada (usuarios/grupos)](https://jackrabbit.apache.org/api/2.8/org/apache/jackrabbit/api/security/user/package-summary.html)
* Observación JCR (escucha de eventos JCR)
* Creación de estructuras de nodos profundos

   * Aunque las API de Sling admiten la creación de recursos, las API de JCR tienen métodos de conveniencia en [JcrUtils](https://jackrabbit.apache.org/api/2.10/index.html?org/apache/jackrabbit/commons/JcrUtils.html) y [JcrUtil](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/commons/jcr/JcrUtil.html) que aceleran la creación de estructuras profundas.

## API de OSGi

* [**OSGi R6 JavaDocs**](https://osgi.org/javadoc/r6/cmpn/index.html?overview-summary.html)
* **[OSGi Declarative Services 1.2 Anotaciones de componentes JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html)**
* **[OSGi Declarative Services 1.2 Anotaciones de tipo de metatipos JavaDocs](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/metatype/annotations/package-summary.html)**
* [**OSGi Framework JavaDocs**](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html)

Hay poca superposición entre las API de OSGi y las API de más alto nivel (AEM, [!DNL Sling] y JCR), y la necesidad de usar las API de OSGi es rara y requiere un alto nivel de experiencia en el desarrollo de AEM.

### OSGi vs API Apache Felix

OSGi define una especificación que todos los contenedores OSGi deben implementar y cumplir. AEM implementación OSGi, Apache Felix, también proporciona varias de sus propias API.

* Prefiera las API de OSGi (`org.osgi`) sobre las API de Apache Felix (`org.apache.felix`).

### Usos comunes de las API de OSGi

* Anotaciones OSGi para declarar servicios y componentes OSGi.

   * Prefiere [OSGi Declarative Services (DS) 1.2 Anotaciones](https://osgi.org/javadoc/r6/cmpn/org/osgi/service/component/annotations/package-summary.html) en lugar de [Anotaciones Félix SCR](https://felix.apache.org/documentation/subprojects/apache-felix-maven-scr-plugin/scr-annotations.html) para declarar servicios y componentes OSGi

* API de OSGi para [anular/registrar dinámicamente servicios/componentes de OSGi](https://osgi.org/javadoc/r6/core/org/osgi/framework/package-summary.html) en código.

   * Prefiera el uso de anotaciones OSGi DS 1.2 cuando no se necesite la administración condicional de componentes o servicios OSGi (lo que suele suceder).

## Excepciones a la regla

Las siguientes son excepciones comunes a las reglas definidas anteriormente.

### API de recursos AEM

* Prefiera [ `com.day.cq.dam.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/day/cq/dam/api/package-summary.html) sobre [ `com.adobe.granite.asset.api`](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/asset/api/package-summary.html).

   * Mientras que las API de `com.day.cq` recursos proporcionan herramientas más gratuitas para AEM casos de uso de administración de recursos.
   * Las API de Granite Assets admiten casos de uso de administración de recursos de bajo nivel (versión, relaciones).

### API de consulta

* AEM QueryBuilder no admite ciertas funciones de consulta como [sugerencias](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#Suggestions), revisión de ortografía y sugerencias de índice, entre otras funciones menos comunes. Para la consulta con estas funciones es preferible JCR-SQL2.

### [!DNL Sling] Registro del servlet  {#sling-servlet-registration}

* [!DNL Sling] registro de servlet, prefiera anotaciones  [OSGi DS 1.2 con @](https://sling.apache.org/documentation/the-sling-engine/servlets.html) SlingServletResourceTypesover  `@SlingServlet`

### [!DNL Sling] Registro de filtros  {#sling-filter-registration}

* [!DNL Sling] registro del filtro, prefiera anotaciones  [OSGi DS 1.2 con @](https://sling.apache.org/documentation/the-sling-engine/filters.html) SlingServletFilterover  `@SlingFilter`

## Recortes de código útiles

Los siguientes son fragmentos de código Java útiles que ilustran las optimizaciones para casos de uso común usando API analizadas. Estos fragmentos también ilustran cómo pasar de las API menos preferidas a las más preferidas.

### Sesión JCR a [!DNL Sling] ResourceResolver

#### Cerrar automáticamente Sling ResourceResolver

Desde AEM 6.2, el [!DNL Sling] ResourceResolver es `AutoClosable` en una sentencia [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html). Con esta sintaxis, no se necesita una llamada explícita a `resourceResolver .close()`.

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

`DamUtil.resolveToAsset(..)`resuelve cualquier recurso debajo del  `dam:Asset` objeto Asset recorriendo el árbol según sea necesario.

```java
Asset asset = DamUtil.resolveToAsset(resource);
```

#### Enfoque alternativo

La adaptación de un recurso a un recurso requiere que el recurso mismo sea el nodo `dam:Asset`.

```java
Asset asset = resource.adaptTo(Asset.class);
```

### [!DNL Sling] Recurso a AEM página

#### Enfoque recomendado

`pageManager.getContainingPage(..)` resuelve cualquier recurso debajo del objeto  `cq:Page` a la página recorriendo el árbol según sea necesario.

```java
PageManager pageManager = resourceResolver.adaptTo(PageManager.class);
Page page = pageManager.getContainingPage(resource);
Page page2 = pageManager.getContainingPage("/content/path/to/page/jcr:content/or/component");
```

#### Enfoque alternativo {#alternative-approach-1}

La adaptación de un recurso a una página requiere que el recurso mismo sea el nodo `cq:Page`.

```java
Page page = resource.adaptTo(Page.class);
```

### Leer propiedades de AEM página

Utilice los captadores del objeto Page para obtener propiedades conocidas (`getTitle()`, `getDescription()`, etc.) y `page.getProperties()` para obtener el `[cq:Page]/jcr:content` ValueMap para recuperar otras propiedades.

```java
Page page = resource.adaptTo(Page.class);
String title = page.getTitle();
Calendar value = page.getProperties().get("cq:lastModified", Calendar.getInstance());
```

### Leer propiedades de metadatos de AEM recurso

La API de recursos proporciona métodos prácticos para leer propiedades desde el nodo `[dam:Asset]/jcr:content/metadata`. Tenga en cuenta que no se trata de un ValueMap; no se admite el segundo parámetro (valor predeterminado y conversión de tipo automático).

```java
Asset asset = resource.adaptTo(Asset.class);
String title = asset.getMetadataValue("dc:title");
Calendar lastModified = (Calendar) asset.getMetadata("cq:lastModified");
```

### Leer [!DNL Sling] [!DNL Resource] propiedades {#read-sling-resource-properties}

Cuando las propiedades se almacenan en ubicaciones (propiedades o recursos relativos) en las que las API de AEM (Página, Recurso) no pueden acceder directamente, se pueden utilizar los [!DNL Sling] Recursos y ValueMaps para obtener los datos.

```java
ValueMap properties = resource.getValueMap();
String value = properties.get("jcr:title", "Default title");
String relativeResourceValue = properties.get("relative/propertyName", "Default value");
```

En este caso, es posible que el objeto AEM tenga que convertirse en [!DNL Sling] [!DNL Resource] para ubicar eficazmente la propiedad o subrecurso que desee.

#### AEM página a [!DNL Sling] [!DNL Resource]

```java
Resource resource = page.adaptTo(Resource.class);
```

#### AEM recurso a [!DNL Sling] [!DNL Resource]

```java
Resource resource = asset.adaptTo(Resource.class);
```

### Escribir propiedades mediante el parámetro ModificableValueMap de [!DNL Sling]

Utilice [!DNL Sling] [ModifiedValueMap](https://sling.apache.org/apidocs/sling10/org/apache/sling/api/resource/ModifiableValueMap.html) para escribir propiedades en los nodos. Esto solo puede escribir en el nodo inmediato (no se admiten rutas de propiedad relativas).

Tenga en cuenta que la llamada a `.adaptTo(ModifiableValueMap.class)` requiere permisos de escritura en el recurso, de lo contrario devolverá null.

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

### Crear un recurso [!DNL Sling]

ResourceResolver admite operaciones básicas para crear recursos. Al crear abstracciones de nivel superior (AEM páginas, recursos, etiquetas, etc.) utilizar los métodos proporcionados por sus respectivos administradores.

```java
resourceResolver.create(parentResource, "my-node-name", new ImmutableMap.Builder<String, Object>()
           .put("jcr:primaryType", "nt:unstructured")
           .put("jcr:title", "Hello world")
           .put("propertyName", "Other initial properties")
           .build());

resourceResolver.commit();
```

### Eliminar un [!DNL Sling] recurso

ResourceResolver admite la eliminación de un recurso. Al crear abstracciones de nivel superior (páginas de AEM, recursos, etiquetas, etc.) utilizar los métodos proporcionados por sus respectivos administradores.

```java
resourceResolver.delete(resource);

resourceResolver.commit();
```
