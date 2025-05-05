---
title: Pruebas unitarias
description: Implemente una prueba unitaria que valide el comportamiento del modelo Sling del componente Byline, creado en el tutorial Componente personalizado.
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
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# Pruebas unitarias {#unit-testing}

Este tutorial cubre la implementación de una prueba unitaria que valida el comportamiento del modelo Sling del componente Byline, creado en el tutorial [Componente personalizado](./custom-component.md).

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

_Si Java™ 8 y Java™ 11 están instalados en el sistema, el ejecutor de pruebas de código VS puede elegir el tiempo de ejecución de Java™ inferior al ejecutar las pruebas, lo que da como resultado errores de prueba. Si esto ocurre, desinstale Java™ 8._

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la rama `tutorial/unit-testing-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Implemente una base de código en una instancia de AEM local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) o desprotegerlo localmente cambiando a la rama `tutorial/unit-testing-start`.

## Objetivo

1. Comprender los conceptos básicos de las pruebas unitarias.
1. Obtenga información sobre los marcos de trabajo y las herramientas que se utilizan normalmente para probar el código de AEM.
1. Comprenda las opciones para burlarse o simular recursos de AEM al escribir pruebas unitarias.

## Fondo {#unit-testing-background}

En este tutorial, exploraremos cómo escribir [pruebas unitarias](https://en.wikipedia.org/wiki/Unit_testing) para el [modelo Sling](https://sling.apache.org/documentation/bundles/models.html) de nuestro componente Byline (creado en [Creación de un componente AEM personalizado](custom-component.md)). Las pruebas unitarias son pruebas en tiempo de compilación escritas en Java™ que verifican el comportamiento esperado del código Java™. Cada prueba unitaria suele ser pequeña y valida el resultado de un método (o unidades de trabajo) con los resultados esperados.

Utilizamos las prácticas recomendadas de AEM y utilizamos:

* [JUnit 5](https://junit.org/junit5/)
* [Marco de prueba Mockito](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/) (que se basa en [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## Pruebas unitarias y Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=es) integra la ejecución de pruebas unitarias y la creación de informes de cobertura de código [3&rbrace; en su canalización de CD/CI para ayudar a alentar y promover las prácticas recomendadas de prueba de unidades de código AEM.](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html?lang=es)

Aunque el código de prueba unitaria es una buena práctica para cualquier base de código, al utilizar Cloud Manager es importante aprovechar sus funciones de prueba de calidad de código y generación de informes al proporcionar pruebas unitarias para que Cloud Manager se ejecute.

## Actualizar las dependencias Maven de prueba {#inspect-the-test-maven-dependencies}

El primer paso es inspeccionar las dependencias de Maven para que admitan la escritura y ejecución de las pruebas. Se requieren cuatro dependencias:

1. JUnit5
1. Marco de prueba de Mockito
1. Apache Sling se burla
1. AEM Mocks Test Framework (de io.wcm)

Las dependencias de prueba **JUnit5**, **Mockito y &#x200B;** AEM Mocks** se agregan automáticamente al proyecto durante la configuración mediante el [arquetipo de AEM Maven](project-setup.md).

1. Para ver estas dependencias, abra el POM del reactor principal en **aem-guides-wknd/pom.xml**, vaya a `<dependencies>..</dependencies>` y vea las dependencias para las pruebas simuladas de JUnit, Mockito, Apache Sling y AEM de io.wcm en `<!-- Testing -->`.
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
   > El tipo de archivo **35** genera el proyecto con `io.wcm.testing.aem-mock.junit5` versión **4.1.8**. Cambie a **4.1.0** para seguir con el resto de este capítulo.

1. Abra **aem-guides-wknd/core/pom.xml** y vea que las dependencias de prueba correspondientes están disponibles.

   Una carpeta de origen paralela en el proyecto **core** contendrá las pruebas unitarias y cualquier archivo de prueba auxiliar. Esta carpeta **test** proporciona separación de las clases de prueba del código fuente, pero permite que las pruebas actúen como si vivieran en los mismos paquetes que el código fuente.

## Creación de la prueba JUnit {#creating-the-junit-test}

Las pruebas unitarias suelen asignar clases de 1 a 1 con Java™. En este capítulo, escribiremos una prueba JUnit para **BylineImpl.java**, que es el modelo Sling que respalda el componente Byline.

![Carpeta src de prueba unitaria](assets/unit-testing/core-src-test-folder.png)

*Ubicación donde se almacenan las pruebas unitarias.*

1. Cree una prueba unitaria para `BylineImpl.java` creando una nueva clase Java™ en `src/test/java` en una estructura de carpetas de paquetes Java™ que refleje la ubicación de la clase Java™ que se va a probar.

   ![Crear un nuevo archivo BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Ya que estamos probando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   cree una clase Java™ de prueba unitaria correspondiente en

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   El sufijo `Test` del archivo de prueba unitaria, `BylineImplTest.java`, es una convención que nos permite

   1. Identifíquelo fácilmente como el archivo de prueba _para_ `BylineImpl.java`
   1. Pero también, diferencie el archivo de prueba _de_ la clase que se está probando, `BylineImpl.java`

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

   Para empezar, comenzamos con un único método de prueba para cada método público en la clase que estamos probando, así que:

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
Como se esperaba, todas las pruebas fallan, ya que aún no se han implementado.

   ![Ejecutar como prueba conjunta](assets/unit-testing/run-junit-tests.png)

   *Haga clic con el botón derecho en BylineImplTests.java > Ejecutar*

## Revisión de BylineImpl.java {#reviewing-bylineimpl-java}

Al escribir pruebas unitarias, existen dos enfoques principales:

* [Desarrollo controlado por pruebas o TDD](https://en.wikipedia.org/wiki/Test-driven_development), que implica escribir las pruebas unitarias de forma incremental, inmediatamente antes de que se desarrolle la implementación; escriba una prueba y escriba la implementación para que la prueba se apruebe.
* Implementación: primero desarrollo, que implica desarrollar primero el código de trabajo y, a continuación, escribir pruebas que validen dicho código.

En este tutorial, se usa el método más reciente (ya que hemos creado un **BylineImpl.java** en funcionamiento en un capítulo anterior). Debido a esto, debemos revisar y comprender los comportamientos de sus métodos públicos, pero también algunos de sus detalles de implementación. Esto puede sonar contrario, ya que una buena prueba solo debe preocuparse por las entradas y salidas, pero al trabajar en AEM, hay varias consideraciones de implementación que deben entenderse para construir pruebas de trabajo.

TDD en el contexto de AEM requiere un nivel de experiencia y es mejor adoptado por los desarrolladores de AEM con experiencia en el desarrollo de AEM y pruebas de unidades de código AEM.

## Configuración del contexto de prueba de AEM  {#setting-up-aem-test-context}

La mayoría del código escrito para AEM se basa en las API de JCR, Sling o AEM, que a su vez requieren el contexto de una AEM en ejecución para ejecutarse correctamente.

Dado que las pruebas unitarias se ejecutan durante la compilación, no existe ese contexto fuera del contexto de una instancia de AEM en ejecución. Para facilitarle esto, [wcm.io&#39;s AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) crea contexto ficticio que permite que estas API _en su mayoría_ actúen como si se estuvieran ejecutando en AEM.

1. Cree un contexto de AEM usando **wcm.io** `AemContext` en **BylineImplTest.java** agregándolo como una extensión JUnit decorada con `@ExtendWith` al archivo **BylineImplTest.java**. La extensión se encarga de todas las tareas de inicialización y limpieza necesarias. Cree una variable de clase para `AemContext` que se pueda usar para todos los métodos de prueba.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Esta variable, `ctx`, expone un contexto de AEM ficticio que proporciona algunas abstracciones de AEM y Sling:

   * El modelo Sling BylineImpl está registrado en este contexto
   * Las estructuras de contenido JCR simuladas se crean en este contexto
   * Los servicios OSGi personalizados se pueden registrar en este contexto
   * Proporciona varios objetos de prueba y ayudantes comunes necesarios, como objetos SlingHttpServletRequest, varios servicios OSGi de Sling y AEM de prueba, como ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag, etc.
      * *No se han implementado todos los métodos para estos objetos.*
   * ¡Y [mucho más](https://wcm.io/testing/aem-mock/usage.html)!

   El objeto **`ctx`** actuará como punto de entrada para la mayor parte de nuestro contexto ficticio.

1. En el método `setUp(..)`, que se ejecuta antes de cada método `@Test`, defina un estado de prueba de simulación común:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra el modelo Sling que se va a probar en el contexto de AEM ficticio para que se pueda crear una instancia en los métodos `@Test`.
   * **`load().json`** carga estructuras de recursos en el contexto ficticio, lo que permite que el código interactúe con estos recursos como si fueran proporcionados por un repositorio real. Las definiciones de recursos del archivo **`BylineImplTest.json`** se cargan en el contexto JCR ficticio en **/content**.
   * **`BylineImplTest.json`** aún no existe, así que vamos a crearlo y definir las estructuras de recursos JCR necesarias para la prueba.

1. Los archivos JSON que representan las estructuras de recursos ficticias se almacenan en **core/src/test/resources**, siguiendo la misma ruta de paquete que el archivo de prueba JUnit Java™.

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

   Este JSON define un recurso ficticio (nodo JCR) para la prueba unitaria de componentes Byline. En este punto, el JSON tiene el conjunto mínimo de propiedades necesarias para representar un recurso de contenido de componente Byline, `jcr:primaryType` y `sling:resourceType`.

   Una regla general al trabajar con pruebas unitarias es crear el conjunto mínimo de contenido, contexto y código ficticio necesario para satisfacer cada prueba. Evite la tentación de crear un contexto ficticio completo antes de escribir las pruebas, ya que a menudo da como resultado artefactos innecesarios.

   Ahora, con la existencia de **BylineImplTest.json**, cuando se ejecuta `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`, las definiciones de recursos ficticias se cargan en el contexto en la ruta de acceso **/content.**

## Prueba de getName() {#testing-get-name}

Ahora que tenemos una configuración básica de contexto ficticio, vamos a escribir nuestra primera prueba para **BylineImpl&#39;s getName()**. Esta prueba debe garantizar que el método **getName()** devuelva el nombre creado correctamente almacenado en la propiedad &quot;**name&quot;** del recurso.

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

   * **`String expected`** establece el valor esperado. Se establecerá en &quot;**Jane Listo**&quot;.
   * **`ctx.currentResource`** establece el contexto del recurso ficticio con el que se evaluará el código, por lo que se establece en **/content/byline**, ya que ahí es donde se carga el recurso de contenido ficticio.
   * **`Byline byline`** crea una instancia del modelo Sling de firma electrónica adaptándolo desde el objeto de solicitud ficticia.
   * **`String actual`** invoca el método que estamos probando, `getName()`, en el objeto del modelo Sling de firma.
   * **`assertEquals`** afirma que el valor esperado coincide con el valor devuelto por el objeto del modelo Sling de firma. Si estos valores no son iguales, la prueba fallará.

1. Ejecute la prueba... y se produce un error con `NullPointerException`.

   Esta prueba NO falla porque nunca definimos una propiedad `name` en el JSON de prueba, lo que provocará que la prueba falle, aunque la ejecución de la prueba no haya llegado a ese punto. Esta prueba falla debido a un `NullPointerException` en el propio objeto de firma.

1. En `BylineImpl.java`, si `@PostConstruct init()` genera una excepción, se evita que el modelo Sling cree una instancia y se hace que el objeto del modelo Sling sea nulo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Resulta que aunque el servicio OSGi de ModelFactory se proporciona a través de `AemContext` (a través de Apache Sling Context), no todos los métodos están implementados, incluido `getModelFromWrappedRequest(...)` al que se llama en el método `init()` de BylineImpl. Esto da como resultado un [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), que a término hace que `init()` falle y que la adaptación resultante de `ctx.request().adaptTo(Byline.class)` sea un objeto nulo.

   Dado que los simulacros proporcionados no pueden acomodar nuestro código, debemos implementar el contexto de simulación nosotros mismos. Para esto, podemos usar Mockito para crear un objeto ModelFactory de simulación, que devuelva un objeto Image de simulación cuando se invoque `getModelFromWrappedRequest(...)` sobre él.

   Dado que para crear una instancia del modelo Sling de firma, este contexto ficticio debe estar configurado, podemos agregarlo al método `@Before setUp()`. También necesitamos agregar `MockitoExtension.class` a la anotación `@ExtendWith` sobre la clase **BylineImplTest**.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca la clase de caso de prueba para que se ejecute con la [Extensión Jupiter Mockito JUnit](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html), que permite el uso de las anotaciones @Mock para definir objetos de prueba en el nivel de clase.
   * **`@Mock private Image`** crea un objeto ficticio de tipo `com.adobe.cq.wcm.core.components.models.Image`. Se define en el nivel de clase para que, según sea necesario, los métodos `@Test` puedan modificar su comportamiento según sea necesario.
   * **`@Mock private ModelFactory`** crea un objeto ficticio de tipo ModelFactory. Esta es una burla de Mockito pura y no tiene métodos implementados. Se define en el nivel de clase para que, según sea necesario, `@Test`los métodos puedan modificar su comportamiento según sea necesario.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra un comportamiento ficticio cuando se llama a `getModelFromWrappedRequest(..)` en el objeto ModelFactory ficticio. El resultado definido en `thenReturn (..)` es devolver el objeto de imagen ficticia. Este comportamiento solo se invoca cuando: el primer parámetro es igual al objeto de solicitud de `ctx`, el segundo parámetro es cualquier objeto Resource y el tercer parámetro debe ser la clase de imagen de los componentes principales. Aceptamos cualquier recurso porque en todas nuestras pruebas establecemos `ctx.currentResource(...)` en varios recursos de prueba definidos en **BylineImplTest.json**. Tenga en cuenta que agregamos la rigidez **lenient()** porque más adelante desearemos anular este comportamiento de ModelFactory.
   * **`ctx.registerService(..)`.** registra el objeto ModelFactory ficticio en AemContext, con la clasificación de servicio más alta. Esto es necesario porque la ModelFactory utilizada en `init()` de BylineImpl se inserta a través del campo `@OSGiService ModelFactory model`. Para que AemContext inserte **nuestro** objeto de simulación, que administra las llamadas a `getModelFromWrappedRequest(..)`, debemos registrarlo como el servicio de mayor clasificación de ese tipo (ModelFactory).

1. Vuelva a ejecutar la prueba y, de nuevo, fallará, pero esta vez el mensaje aclara por qué ha fallado.

   ![afirmación de error de nombre de prueba](assets/unit-testing/testgetname-failure-assertion.png)

   Error de *testGetName() debido a la afirmación*

   Recibimos un **AssertionError** que significa que la condición de aserción en la prueba falló y nos dice que el valor **esperado es &quot;Jane Doe&quot;** pero el **valor real es nulo**. Esto tiene sentido porque la propiedad &quot;**name&quot;** no se ha agregado a la definición de recurso **/content/byline** simulada en **BylineImplTest.json**, así que vamos a agregarla:

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

   ![nombre de prueba superado](assets/unit-testing/testgetname-pass.png)


## Prueba de getOccupations() {#testing-get-occupations}

¡Muy bien! ¡La primera prueba ha pasado! Vamos a pasar página y probar `getOccupations()`. Dado que la inicialización del contexto ficticio se realizó en el método `@Before setUp()`, esto está disponible para todos los métodos `@Test` en este caso de prueba, incluido `getOccupations()`.

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
   * **`ctx.currentResource`** establece el recurso actual para evaluar el contexto con respecto a la definición del recurso ficticio en /content/byline. Esto garantiza que **BylineImpl.java** se ejecute en el contexto de nuestro recurso ficticio.
   * **`ctx.request().adaptTo(Byline.class)`** crea una instancia del modelo Sling de firma electrónica adaptándolo desde el objeto de solicitud ficticia.
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

1. ¡Corre la prueba, y de nuevo pasamos! ¡Parece que conseguir las ocupaciones ordenadas funciona!

   ![Obtener ocupación aprobado](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() supera*

## Prueba isEmpty() {#testing-is-empty}

El último método para probar **`isEmpty()`**.

La prueba `isEmpty()` es interesante ya que requiere pruebas para diversas condiciones. Revisando el método `isEmpty()` de **BylineImpl.java** se deben probar las siguientes condiciones:

* Devolver verdadero cuando el nombre está vacío
* Devolver verdadero cuando las ocupaciones son nulas o están vacías
* Devolver verdadero cuando la imagen es nula o no tiene URL de origen
* Devuelve &quot;false&quot; cuando el nombre, las ocupaciones y la imagen (con una URL src) están presentes

Para ello, necesitamos crear métodos de prueba, probar cada uno una condición específica y nuevas estructuras de recursos ficticios en `BylineImplTest.json` para controlar estas pruebas.

Esta comprobación nos permitió omitir la prueba para cuando `getName()`, `getOccupations()` y `getImage()` están vacíos, ya que el comportamiento esperado de ese estado se prueba mediante `isEmpty()`.

1. La primera prueba probará la condición de un componente completamente nuevo que no tiene propiedades establecidas.

   Agregue una nueva definición de recurso a `BylineImplTest.json`, asignándole el nombre semántico &quot;**empty**&quot;

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

   **`"empty": {...}`** define una nueva definición de recurso denominada &quot;vacía&quot; que solo tiene `jcr:primaryType` y `sling:resourceType`.

   Recuerde que cargamos `BylineImplTest.json` en `ctx` antes de la ejecución de cada método de prueba en `@setUp`, por lo que esta nueva definición de recurso está disponible de inmediato para nosotros en las pruebas de **/content/empty.**

1. Actualice `testIsEmpty()` de la siguiente manera, estableciendo el recurso actual en la nueva definición de recurso de imitación &quot;**empty**&quot;.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Ejecute la prueba y asegúrese de que pasa.

1. A continuación, cree un conjunto de métodos para asegurarse de que si alguno de los puntos de datos necesarios (nombre, ocupaciones o imagen) está vacío, `isEmpty()` devuelve true.

   Para cada prueba, se usa una definición de recurso ficticio discreta, actualice **BylineImplTest.json** con las definiciones de recurso adicionales para **sin nombre** y **sin ocupaciones**.

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

   **`testIsEmpty()`** comprueba la definición de recurso ficticio vacío y afirma que `isEmpty()` es verdadero.

   **`testIsEmpty_WithoutName()`** prueba una definición de recurso ficticio que tiene ocupaciones pero no tiene nombre.

   **`testIsEmpty_WithoutOccupations()`** prueba una definición de recurso ficticio que tiene un nombre pero ninguna ocupación.

   **`testIsEmpty_WithoutImage()`** prueba una definición de recurso ficticio con un nombre y ocupaciones, pero establece la imagen ficticia como nula. Tenga en cuenta que queremos anular el comportamiento `modelFactory.getModelFromWrappedRequest(..)` definido en `setUp()` para garantizar que el objeto de imagen devuelto por esta llamada sea nulo. La función de código auxiliar de Mockito es estricta y no desea código duplicado. Por lo tanto, establecemos la configuración de simulación con **`lenient`** para indicar explícitamente que se está anulando el comportamiento en el método `setUp()`.

   **`testIsEmpty_WithoutImageSrc()`** prueba una definición de recurso ficticio con un nombre y ocupaciones, pero establece la imagen ficticia para que devuelva una cadena en blanco cuando se invoca a `getSrc()`.

1. Por último, escriba una prueba para asegurarse de que **isEmpty()** devuelve false cuando el componente esté configurado correctamente. Para esta condición, podemos reutilizar **/content/byline**, que representa un componente Byline completamente configurado.

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

![Todas las pruebas superan](./assets/unit-testing/all-tests-pass.png)

## Ejecución de pruebas unitarias como parte de la compilación {#running-unit-tests-as-part-of-the-build}

Las pruebas unitarias se ejecutan y son necesarias para pasarlas como parte de la compilación de Maven. Esto garantiza que todas las pruebas pasen correctamente antes de implementar una aplicación. La ejecución de los objetivos de Maven, como empaquetar o instalar, invoca automáticamente y requiere que se pasen todas las pruebas unitarias del proyecto.

```shell
$ mvn package
```

![paquete mvn correcto](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Del mismo modo, si se cambia un método de prueba a failed, la compilación falla y se informa de qué prueba ha fallado y por qué.

![error del paquete mvn](assets/unit-testing/mvn-package-fail.png)

## Revise el código {#review-the-code}

Vea el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/unit-testing-solution`.
