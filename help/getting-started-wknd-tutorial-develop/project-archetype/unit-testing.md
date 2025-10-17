---
title: Pruebas de unidad
description: Implemente una prueba de unidad que valide el comportamiento del modelo Sling del componente de firma, creado en el tutorial Componente personalizado.
version: Experience Manager 6.5, Experience Manager as a Cloud Service
feature: APIs, AEM Project Archetype
topic: Content Management, Development
role: Developer
level: Beginner
jira: KT-4089
mini-toc-levels: 1
thumbnail: 30207.jpg
doc-type: Tutorial
exl-id: b926c35e-64ad-4507-8b39-4eb97a67edda
recommendations: noDisplay, noCatalog
duration: 706
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '2923'
ht-degree: 100%

---

# Pruebas de unidad {#unit-testing}

Este tutorial cubre la implementación de una prueba de unidad que valida el comportamiento del modelo Sling del componente de firma, creado en el tutorial [Componente personalizado](./custom-component.md).

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

_Si Java™ 8 y Java™ 11 están instalados en el sistema, el ejecutor de pruebas de código VS puede elegir el tiempo de ejecución de Java™ inferior al ejecutar las pruebas, lo que se traduce en errores de prueba. Si esto ocurre, desinstale Java™ 8._

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para consultar el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la rama `tutorial/unit-testing-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Implemente una base de código en una instancia de AEM local usando sus conocimientos de Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) o consultarlo localmente cambiando a la rama `tutorial/unit-testing-start`.

## Objetivo

1. Conocer los conceptos básicos de las pruebas de unidad.
1. Obtener información sobre los marcos de trabajo y las herramientas que se utilizan normalmente para probar el código de AEM.
1. Conocer las opciones para simular recursos de AEM al escribir pruebas de unidad.

## Información general {#unit-testing-background}

En este tutorial, exploraremos cómo escribir [pruebas de unidad](https://es.wikipedia.org/wiki/Unit_testing) para el [modelo Sling](https://sling.apache.org/documentation/bundles/models.html?lang=es) del componente de Firma (generado en [Creación de un componente AEM personalizado](custom-component.md)). Las pruebas de unidad son pruebas en tiempo de compilación escritas en Java™ que verifican el comportamiento previsto del código Java™. Cada prueba de unidad suele ser pequeña y valida el resultado de un método (o unidades de trabajo) comparándolos con los resultados esperados.

Utilizamos las prácticas recomendadas de AEM y utilizamos:

* [JUnit 5](https://junit.org/junit5/)
* [Marco de trabajo de prueba Mockito](https://site.mockito.org/)
* [Marco de trabajo de prueba wcm.io](https://wcm.io/testing/) (que se basa en [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html?lang=es))

## Pruebas de unidad y Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=es) integra la ejecución de pruebas de unidad y los [sistemas de informes de cobertura de código](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html?lang=es) en su CD/CI Pipeline para ayudar a fomentar y promover la práctica recomendada de prueba de unidad del código AEM.

Aunque la prueba de unidad del código es una buena práctica para cualquier base de código, cuando se utiliza Cloud Manager es importante aprovechar sus funciones de prueba de calidad de código y creación de informes proporcionando pruebas de unidad para que Cloud Manager se ejecute.

## Actualización de las dependencias Maven de prueba {#inspect-the-test-maven-dependencies}

El primer paso es inspeccionar las dependencias de Maven para que admitan la escritura y ejecución de las pruebas. Se requieren cuatro dependencias:

1. JUnit5
1. Marco de trabajo de prueba Mockito
1. Apache Sling Mocks
1. Marco de trabajo de prueba de AEM Mocks (por io.wcm)

Las dependencias de prueba **JUnit5**, **Mockito y **AEM Mocks** se añaden automáticamente al proyecto durante la configuración mediante el [arquetipo de AEM Maven](project-setup.md).

1. Para ver estas dependencias, abra el POM del reactor principal en **aem-guides-wknd/pom.xml**, vaya a `<dependencies>..</dependencies>` y vea las dependencias de las pruebas de JUnit, Mockito, Apache Sling Mocks y AEM Mock realizadas por io.wcm en `<!-- Testing -->`.
1. Asegúrese de que `io.wcm.testing.aem-mock.junit5` está establecido en **4.1.0**:

   ```xml
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <version>4.1.0</version>
       <scope>test</scope>
   </dependency>
   ```

   >[!CAUTION]
   >
   > El arquetipo **35** genera el proyecto con `io.wcm.testing.aem-mock.junit5` versión **4.1.8**. Use la versión inferior **4.1.0** para seguir con el resto de este capítulo.

1. Abra **aem-guides-wknd/core/pom.xml** y vea que las dependencias de prueba correspondientes están disponibles.

   Una carpeta de origen paralela en el proyecto **core** contendrá las pruebas de unidad y cualquier archivo de prueba de soporte. Esta carpeta **test** proporciona la separación de las clases de prueba del código fuente, pero permite que las pruebas actúen como si residieran en los mismos paquetes que el código fuente.

## Creación de la prueba JUnit {#creating-the-junit-test}

Las pruebas de unidad suelen corresponderse una a una con las clases Java™. En este capítulo, escribiremos una prueba JUnit para **BylineImpl.java**, que es el modelo Sling que respalda el componente de firma.

![Carpeta src de prueba de unidad](assets/unit-testing/core-src-test-folder.png)

*Ubicación donde se almacenan las pruebas de unidad.*

1. Cree una prueba de unidad para `BylineImpl.java` creando una nueva clase Java™ en `src/test/java` en una estructura de carpetas de paquetes Java™ que refleje la ubicación de la clase Java™ que se va a probar.

   ![Crear un nuevo archivo BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Ya que estamos probando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   cree una clase Java™ de prueba de unidad correspondiente en

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   El sufijo `Test` del archivo de prueba de unidad, `BylineImplTest.java`, es una convención que nos permite

   1. Identificarlo fácilmente como el archivo de prueba _para_ `BylineImpl.java`
   1. Pero también, diferenciar el archivo de prueba _de_ la clase que se está probando, `BylineImpl.java`

## Revisión de BylineImplTest.java {#reviewing-bylineimpltest-java}

En este punto, el archivo de prueba JUnit es una clase Java™ vacía.

1. Actualice el archivo con el siguiente código:

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import static org.junit.jupiter.api.Assertions.*;
   
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   
   public class BylineImplTest {
   
       @BeforeEach
       void setUp() throws Exception {
   
       }
   
       @Test 
       void testGetName() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testGetOccupations() { 
           fail("Not yet implemented");
       }
   
       @Test 
       void testIsEmpty() { 
           fail("Not yet implemented");
       }
   }
   ```

1. El primer método `public void setUp() { .. }` está anotado con `@BeforeEach` de JUnit, que indica al ejecutor de pruebas de JUnit que ejecute este método antes de ejecutar cada método de prueba en esta clase. Esto proporciona un lugar práctico para inicializar el estado de prueba común requerido por todas las pruebas.

1. Los métodos siguientes son los métodos de prueba, cuyos nombres llevan el prefijo `test` por convención y están marcados con la anotación `@Test`. Tenga en cuenta que, de forma predeterminada, todas nuestras pruebas están configuradas para fallar, ya que aún no las hemos implementado.

   Para empezar, comenzaremos con un único método de prueba para cada método público en la clase que estamos probando, así que:

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | está probado por | testGetName() |
   | getOccupations() | está probado por | testGetOccupations() |
   | isEmpty() | está probado por | testIsEmpty() |

   Estos métodos se pueden ampliar según sea necesario, como veremos más adelante en este capítulo.

   Cuando se ejecuta esta clase de prueba JUnit (también conocida como caso de prueba JUnit), cada método marcado con `@Test` se ejecutará como una prueba que puede aprobar o no.

![BylineImplTest generado](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Ejecute el caso de prueba JUnit haciendo clic con el botón derecho en el archivo `BylineImplTest.java` y pulsando **Ejecutar**.
Tal como se esperaba, todas las pruebas fallan, ya que aún no se han implementado.

   ![Ejecutar como prueba junit](assets/unit-testing/run-junit-tests.png)

   *Haga clic con el botón derecho en BylineImplTests.java > Ejecutar*

## Revisión de BylineImpl.java {#reviewing-bylineimpl-java}

Al escribir pruebas unitarias, existen dos enfoques principales:

* El [Desarrollo controlado por pruebas o TDD](https://es.wikipedia.org/wiki/Test-driven_development) implica escribir las pruebas unitarias de forma incremental, inmediatamente antes de que se desarrolle la implementación; debe escribir una prueba y su implementación para que se apruebe.
* Implementación: primero debe desarrollar el código de trabajo y, a continuación, escribir las pruebas que validen dicho código.

En este tutorial, se usa el método más reciente (ya que, en un capítulo anterior, hemos creado un **BylineImpl.java** que funciona). Debido a esto, debemos revisar y comprender los comportamientos de sus métodos públicos, pero también algunos de los detalles de implementación. Esto puede parecer contradictorio, ya que una buena prueba solo debería ocuparse de las entradas y salidas, pero al trabajar en AEM, hay varias consideraciones de implementación que es necesario comprender para crear pruebas que funcionen.

TDD en el contexto de AEM requiere un nivel de experiencia y es más adecuado para desarrolladores de AEM con experiencia en el desarrollo de AEM y pruebas de unidad de código AEM.

## Configuración del contexto de prueba de AEM  {#setting-up-aem-test-context}

La mayoría del código escrito para AEM se basa en las API de JCR, Sling o AEM, que a su vez requieren el contexto de una AEM en ejecución para ejecutarse correctamente.

Dado que las pruebas unitarias se ejecutan durante la compilación, no existe ese contexto fuera del contexto de una instancia de AEM en ejecución. Para facilitar esto, [wcm.io&#39;s AEM Mocks](https://wcm.io/testing/aem-mock/usage.html?lang=es) crea un contexto de simulación que permite que estas API actúen, _en su mayoría_, como si se estuvieran ejecutando en AEM.

1. Cree un contexto de AEM usando **wcm.io** `AemContext` en **BylineImplTest.java** añadiéndolo como una extensión JUnit decorada con `@ExtendWith` al archivo **BylineImplTest.java**. La extensión se encarga de todas las tareas de inicialización y limpieza necesarias. Cree una variable de clase para `AemContext` que se pueda usar para todos los métodos de prueba.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Esta variable, `ctx`, expone un contexto de AEM simulado que proporciona algunas abstracciones de AEM y Sling:

   * El modelo Sling BylineImpl está registrado en este contexto
   * Las estructuras de contenido JCR simuladas se crean en este contexto
   * Los servicios OSGi personalizados se pueden registrar en este contexto
   * Proporciona varios objetos simulados y ayudantes comunes necesarios, como objetos SlingHttpServletRequest, varios servicios OSGi de Sling y AEM de prueba, como ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag, etc.
      * *No se han implementado todos los métodos para estos objetos.*
   * Y [mucho más](https://wcm.io/testing/aem-mock/usage.html?lang=es).

   El objeto **`ctx`** actuará como punto de entrada para la mayor parte de nuestro contexto simulado.

1. En el método `setUp(..)`, que se ejecuta antes de cada método `@Test`, defina un estado de prueba de simulación común:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra el modelo Sling que se va a probar en el contexto de AEM simulado para que se pueda crear una instancia en los métodos `@Test`.
   * **`load().json`** carga estructuras de recursos en el contexto simulado, lo que permite que el código interactúe con estos recursos como si fueran proporcionados por un repositorio real. Las definiciones de recursos del archivo **`BylineImplTest.json`** se cargan en el contexto JCR simulado en **/content**.
   * **`BylineImplTest.json`** aún no existe, así que vamos a crearlo y definir las estructuras de recursos JCR necesarias para la prueba.

1. Los archivos JSON que representan las estructuras de recursos simuladas se almacenan en **core/src/test/resources**, siguiendo la misma ruta de paquete que el archivo de prueba JUnit Java™.

   Cree un archivo JSON en `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` llamado **BylineImplTest.json** con el siguiente contenido:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Este JSON define un recurso simulado (nodo JCR) para la prueba unitaria de componentes Byline. En este punto, el JSON tiene el conjunto mínimo de propiedades necesarias para representar un recurso de contenido de componente Byline, `jcr:primaryType` y `sling:resourceType`.

   Una regla general al trabajar con pruebas unitarias es crear el conjunto mínimo de contenido, contexto y código simulado necesario para satisfacer cada prueba. Evite la tentación de crear un contexto simulado completo antes de escribir las pruebas, ya que a menudo da como resultado artefactos innecesarios.

   Ahora, con la existencia de **BylineImplTest.json**, cuando se ejecuta `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`, las definiciones de recursos simuladas se cargan en el contexto en la ruta de acceso **/content.**

## Prueba de getName() {#testing-get-name}

Ahora que tenemos una configuración básica de contexto simulado, vamos a escribir nuestra primera prueba para **BylineImpl&#39;s getName()**. Esta prueba debe garantizar que el método **getName()** devuelva el nombre creado correctamente almacenado en la propiedad “**name”** del recurso.

1. Actualice el método **testGetName**() en **BylineImplTest.java** de la siguiente manera:

   ```java
   import com.adobe.aem.guides.wknd.core.models.Byline;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** establece el valor esperado. Se establecerá en “**Jane Done**”.
   * **`ctx.currentResource`** establece el contexto del recurso simulado con el que se evaluará el código, por lo que se establece en **/content/byline**, ya que ahí es donde se carga el recurso de contenido simulado.
   * **`Byline byline`** crea una instancia del modelo Sling de firma electrónica adaptándolo desde el objeto de solicitud ficticia.
   * **`String actual`** invoca el método que estamos probando, `getName()`, en el objeto del modelo Sling de firma.
   * **`assertEquals`** afirma que el valor esperado coincide con el valor devuelto por el objeto del modelo Sling de firma. Si estos valores no son iguales, la prueba fallará.

1. Ejecute la prueba... y se produce un error con un `NullPointerException`.

   Esta prueba NO falla porque nunca definimos una propiedad `name` en el JSON simulado, lo que provocará que la prueba falle, aunque la ejecución de la prueba no haya llegado a ese punto. Esta prueba falla debido a un `NullPointerException` en el propio objeto de firma.

1. En `BylineImpl.java`, si `@PostConstruct init()` genera una excepción, se evita que el modelo Sling cree una instancia y se hace que el objeto del modelo Sling sea nulo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Resulta que aunque el servicio OSGi de ModelFactory se proporciona a través de `AemContext` (a través de Apache Sling Context), no todos los métodos están implementados, incluido `getModelFromWrappedRequest(...)` al que se llama en el método `init()` de BylineImpl. Esto da como resultado un [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), que a término hace que `init()` falle y que la adaptación resultante de `ctx.request().adaptTo(Byline.class)` sea un objeto nulo.

   Dado que los simulacros proporcionados no pueden acomodar nuestro código, debemos implementar el contexto de simulación nosotros mismos. Para esto, podemos usar Mockito para crear un objeto ModelFactory de simulación, que devuelva un objeto Image de simulación cuando se invoque `getModelFromWrappedRequest(...)` sobre él.

   Dado que para incluso crear una instancia del modelo Sling de firma, este contexto ficticio debe estar configurado, podemos añadirlo al método `@Before setUp()`. También necesitamos añadir `MockitoExtension.class` a la anotación `@ExtendWith` sobre la clase **BylineImplTest**.

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.*;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca la clase de caso de prueba para que se ejecute con la [Extensión Jupiter Mockito JUnit](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html?lang=es), que permite el uso de las anotaciones @Mock para definir objetos de prueba en el nivel de clase.
   * **`@Mock private Image`** crea un objeto simulado de tipo `com.adobe.cq.wcm.core.components.models.Image`. Esto se define en el nivel de clase para que, según sea necesario, los métodos `@Test` puedan modificar su comportamiento según sea necesario.
   * **`@Mock private ModelFactory`** crea un objeto ficticio de tipo ModelFactory. Esta es una simulación auténtica de Mockito y no tiene métodos implementados. Se define en el nivel de clase para que, según sea necesario, `@Test`los métodos puedan modificar su comportamiento según sea necesario.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra un comportamiento simulado cuando se invoca a `getModelFromWrappedRequest(..)` en el objeto ModelFactory simulado. El resultado definido en `thenReturn (..)` es devolver el objeto de imagen simulado. Este comportamiento solo se invoca cuando: el primer parámetro es igual al objeto de solicitud de `ctx`, el segundo parámetro es cualquier objeto Resource y el tercer parámetro debe ser la clase de imagen de los componentes principales. Aceptamos cualquier recurso porque en todas nuestras pruebas establecemos `ctx.currentResource(...)` en varios recursos de prueba definidos en **BylineImplTest.json**. Tenga en cuenta que añadimos la rigidez **lenient()** porque más adelante desearemos anular este comportamiento de ModelFactory.
   * **`ctx.registerService(..)`.** registra el objeto ModelFactory simulado en AemContext, con la clasificación de servicio más alta. Esto es necesario porque la ModelFactory utilizada en `init()` de BylineImpl se inserta a través del campo `@OSGiService ModelFactory model`. Para que AemContext inserte **nuestro** objeto de simulación, que administra las llamadas a `getModelFromWrappedRequest(..)`, debemos registrarlo como el servicio de mayor clasificación de ese tipo (ModelFactory).

1. Vuelva a ejecutar la prueba y, de nuevo, se produce un error, pero esta vez el mensaje aclara el motivo del error.

   ![afirmación de error de nombre de prueba](assets/unit-testing/testgetname-failure-assertion.png)

   *Error de testGetName() debido a la afirmación*

   Recibimos un **AssertionError** que significa que la condición de aserción en la prueba falló y nos dice que el **valor esperado es “Jane Doe”** pero el **valor real es nulo**. Esto tiene sentido porque la propiedad “**name”** no se ha añadido a la definición de recurso **/content/byline** simulada en **BylineImplTest.json**, así que vamos a añadirla:

1. Actualizar **BylineImplTest.json** para definir `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. ¡Vuelva a ejecutar la prueba y **`testGetName()`** ahora se aprobará!

   ![nombre de prueba aprobada](assets/unit-testing/testgetname-pass.png)


## Prueba de getOccupations() {#testing-get-occupations}

¡Muy bien! ¡Se ha aprobado la primera prueba! Vamos a continuar y probar `getOccupations()`. Dado que la inicialización del contexto simulado se realizó en el método `@Before setUp()`, esto está disponible para todos los métodos `@Test` en este caso de prueba, incluido `getOccupations()`.

Recuerde que este método debe devolver una lista ordenada alfabéticamente de ocupaciones (descendentes) almacenadas en la propiedad de ocupaciones.

1. Actualice **`testGetOccupations()`** de la siguiente manera:

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** define el resultado esperado.
   * **`ctx.currentResource`** establece el recurso actual para evaluar el contexto con respecto a la definición del recurso simulado en /content/byline. Esto garantiza que **BylineImpl.java** se ejecute en el contexto de nuestro recurso simulado.
   * **`ctx.request().adaptTo(Byline.class)`** crea una instancia del modelo Sling de firma electrónica adaptándolo desde el objeto de solicitud simulada.
   * **`byline.getOccupations()`** invoca el método que estamos probando, `getOccupations()`, en el objeto del modelo Sling de firma.
   * **`assertEquals(expected, actual)`** afirma que la lista esperada es la misma que la lista real.

1. Recuerde, al igual que **`getName()`** más arriba, **BylineImplTest.json** no define ocupaciones, por lo que esta prueba fallará si la ejecutamos, ya que `byline.getOccupations()` devolverá una lista vacía.

   Actualice **BylineImplTest.json** para incluir una lista de ocupaciones, y se establecen en orden no alfabético para garantizar que nuestras pruebas validen que las ocupaciones se ordenan alfabéticamente por **`getOccupations()`**.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. ¡Ejecute la prueba, y obtenemos la aprobación de nuevo! ¡Parece que conseguir las ocupaciones ordenadas funciona!

   ![Obtener aprobación de Ocupaciones](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() se aprueba*

## Prueba isEmpty() {#testing-is-empty}

El último método para realizar la prueba **`isEmpty()`**.

La prueba `isEmpty()` es interesante ya que requiere pruebas para diversas condiciones. Al revisar el método `isEmpty()` de **BylineImpl.java** se deben probar las siguientes condiciones:

* Devolver verdadero cuando el nombre está vacío
* Devolver verdadero cuando las ocupaciones son nulas o están vacías
* Devolver verdadero cuando la imagen es nula o no tiene URL src
* Devuelve “false” cuando el nombre, las ocupaciones y la imagen (con una URL src) están presentes

Para ello, necesitamos crear métodos de prueba, probar cada uno una condición específica y nuevas estructuras de recursos ficticios en `BylineImplTest.json` para controlar estas pruebas.

Esta comprobación nos permitió omitir la prueba para cuando `getName()`, `getOccupations()` y `getImage()` están vacíos, ya que el comportamiento esperado de ese estado se prueba mediante `isEmpty()`.

1. La primera prueba probará la condición de un componente completamente nuevo que no tiene propiedades establecidas.

   Añada una nueva definición de recurso a `BylineImplTest.json`, asignándole el nombre semántico “**empty**”

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** define una nueva definición de recurso denominada “vacía” que solo tiene `jcr:primaryType` y `sling:resourceType`.

   Recuerde que cargamos `BylineImplTest.json` en `ctx` antes de la ejecución de cada método de prueba en `@setUp`, por lo que esta nueva definición de recurso está disponible de inmediato para nosotros en las pruebas de **/content/empty.**

1. Actualice `testIsEmpty()` de la siguiente manera, estableciendo el recurso actual en la nueva definición de recurso simulado ”**empty**”.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Ejecute la prueba y asegúrese de que se aprueba.

1. A continuación, cree un conjunto de métodos para asegurarse de que si alguno de los puntos de datos necesarios (nombre, ocupaciones o imagen) está vacío, `isEmpty()` devuelve true.

   Para cada prueba, se usa una definición de recurso simulado discreta, actualice **BylineImplTest.json** con las definiciones de recurso adicionales para **sin nombre** y **sin ocupaciones**.

   ```json
   {
       "byline": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe",
           "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
           "jcr:primaryType": "nt:unstructured",
           "sling:resourceType": "wknd/components/content/byline",
           "name": "Jane Doe"
       }
   }
   ```

   Cree los siguientes métodos de prueba para probar cada uno de estos estados.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** comprueba la definición de recurso simulado vacío y afirma que `isEmpty()` es verdadero.

   **`testIsEmpty_WithoutName()`** prueba una definición de recurso simulado que tiene ocupaciones pero no tiene nombre.

   **`testIsEmpty_WithoutOccupations()`** prueba una definición de recurso simulado que tiene un nombre pero ninguna ocupación.

   **`testIsEmpty_WithoutImage()`** prueba una definición de recurso simulado con un nombre y ocupaciones, pero establece la imagen simulada como nula. Tenga en cuenta que queremos anular el `modelFactory.getModelFromWrappedRequest(..)`comportamiento definido en `setUp()` para garantizar que el objeto de imagen devuelto por esta petición sea nulo. La función de código auxiliar de Mockito es estricta y no desea código duplicado. Por lo tanto, establecemos la configuración de simulación con **`lenient`** para indicar explícitamente que se está anulando el comportamiento en el método `setUp()`.

   **`testIsEmpty_WithoutImageSrc()`** prueba una definición de recurso simulado con un nombre y ocupaciones, pero establece la imagen ficticia para que devuelva una cadena en blanco cuando se invoca a `getSrc()`.

1. Por último, escriba una prueba para asegurarse de que **isEmpty()** devuelve false cuando el componente está configurado correctamente. Para esta condición, podemos reutilizar **/content/byline**, que representa un componente Byline completamente configurado.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Ahora ejecute todas las pruebas unitarias en el archivo BylineImplTest.java y revise el resultado del informe de prueba de Java™.

![Todas las pruebas aprobadas](./assets/unit-testing/all-tests-pass.png)

## Ejecución de pruebas unitarias como parte de la compilación {#running-unit-tests-as-part-of-the-build}

Las pruebas unitarias se ejecutan y es obligatorio que se aprueben como parte de la compilación de Maven. Esto garantiza que todas las pruebas se aprueben correctamente antes de implementar una aplicación. La ejecución de los objetivos de Maven, como empaquetar o instalar, invoca automáticamente y requiere que se aprueben todas las pruebas unitarias del proyecto.

```shell
$ mvn package
```

![paquete mvn correcto](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Del mismo modo, si se cambia un método de prueba a fallido, la compilación falla y se informa de qué prueba ha fallado y por qué.

![error del paquete mvn](assets/unit-testing/mvn-package-fail.png)

## Revisar el código {#review-the-code}

Ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revisar e implementar el código localmente en la rama Git `tutorial/unit-testing-solution`.
