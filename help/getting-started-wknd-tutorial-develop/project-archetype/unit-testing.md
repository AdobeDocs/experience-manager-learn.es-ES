---
title: Pruebas unitarias
description: Implemente una prueba unitaria que valide el comportamiento del modelo Sling del componente Byline, creado en el tutorial Componente personalizado.
version: 6.5, Cloud Service
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
duration: 851
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 0%

---

# Pruebas unitarias {#unit-testing}

Este tutorial cubre la implementación de una prueba unitaria que valida el comportamiento del modelo Sling del componente Byline, creado en [Componente personalizado](./custom-component.md) tutorial.

## Requisitos previos {#prerequisites}

Revise las herramientas y las instrucciones necesarias para configurar una [entorno de desarrollo local](overview.md#local-dev-environment).

_Si Java™ 8 y Java™ 11 están instalados en el sistema, el ejecutor de pruebas de código VS puede elegir el tiempo de ejecución de Java™ inferior al ejecutar las pruebas, lo que da como resultado errores de prueba. Si esto sucede, desinstale Java™ 8._

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para desproteger el proyecto de inicio.

Consulte el código de línea de base en el que se basa el tutorial:

1. Consulte la `tutorial/unit-testing-start` bifurcar desde [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. AEM Implemente una base de código en una instancia de local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > AEM Si se utiliza la versión 6.5 o 6.4, se debe anexar la variable `classic` de perfil a cualquier comando de Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) o compruebe el código localmente cambiando a la rama `tutorial/unit-testing-start`.

## Objetivo

1. Comprender los conceptos básicos de las pruebas unitarias.
1. AEM Obtenga información sobre los marcos de trabajo y las herramientas que se utilizan normalmente para probar el código de.
1. AEM Comprender las opciones para burlarse o simular recursos de la unidad al escribir pruebas de unidad.

## Fondo {#unit-testing-background}

En este tutorial, vamos a explorar cómo escribir [Pruebas unitarias](https://en.wikipedia.org/wiki/Unit_testing) para el componente de Firma [Modelo Sling](https://sling.apache.org/documentation/bundles/models.html) (creado en el [AEM Creación de un componente personalizado de la](custom-component.md)). Las pruebas unitarias son pruebas en tiempo de compilación escritas en Java™ que verifican el comportamiento esperado del código Java™. Cada prueba unitaria suele ser pequeña y valida el resultado de un método (o unidades de trabajo) con los resultados esperados.

AEM Utilizamos las prácticas recomendadas de la y utilizamos:

* [JUnit 5](https://junit.org/junit5/)
* [Marco de prueba de Mockito](https://site.mockito.org/)
* [Marco de prueba wcm.io](https://wcm.io/testing/) (que se basa en [Apache Sling se burla](https://sling.apache.org/documentation/development/sling-mock.html))

## Pruebas unitarias y Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=es) integra la ejecución de pruebas unitarias y [informes de cobertura de código](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html) AEM en su canalización de CI/CD para ayudar a alentar y promover las prácticas recomendadas de prueba de unidades de código de la.

Aunque el código de prueba unitaria es una buena práctica para cualquier base de código, al utilizar Cloud Manager es importante aprovechar sus instalaciones de prueba de calidad de código y generación de informes al proporcionar pruebas unitarias para que Cloud Manager se ejecute.

## Actualizar las dependencias Maven de prueba {#inspect-the-test-maven-dependencies}

El primer paso es inspeccionar las dependencias de Maven para que admitan la escritura y ejecución de las pruebas. Se requieren cuatro dependencias:

1. JUnit5
1. Marco de prueba de Mockito
1. Apache Sling se burla
1. AEM Marco de prueba de Mocks (por io.wcm)

El **JUnit5**, **Mockito y **AEM** las dependencias de prueba se agregan automáticamente al proyecto durante la configuración utilizando [AEM Arquetipo de Maven](project-setup.md).

1. Para ver estas dependencias, abra el POM de Reactor principal en **aem-guides-wknd/pom.xml**, vaya al `<dependencies>..</dependencies>` AEM y vea las dependencias para las pruebas de JUnit, Mockito, Apache Sling Mocks y Mock de la versión de i.wcm en, que se encuentran en `<!-- Testing -->`.
1. Asegúrese de que `io.wcm.testing.aem-mock.junit5` se establece en **4.1.0**:

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
   > Archetype **35** genera el proyecto con `io.wcm.testing.aem-mock.junit5` version **4.1.8**. Cambie a **4.1.0** para seguir el resto de este capítulo.

1. Abrir **aem-guides-wknd/core/pom.xml** y ver que las dependencias de prueba correspondientes están disponibles.

   Una carpeta de origen paralela en el **núcleo** project contendrá las pruebas unitarias y los archivos de prueba auxiliares. Esta **prueba** proporciona separación entre las clases de prueba y el código fuente, pero permite que las pruebas actúen como si estuvieran en los mismos paquetes que el código fuente.

## Creación de la prueba JUnit {#creating-the-junit-test}

Las pruebas unitarias suelen asignar clases de 1 a 1 con Java™. En este capítulo, escribiremos una prueba JUnit para el **BylineImpl.java**, que es el Modelo Sling que respalda el componente Firma.

![Carpeta src de prueba unitaria](assets/unit-testing/core-src-test-folder.png)

*Ubicación donde se almacenan las pruebas unitarias.*

1. Cree una prueba unitaria para el `BylineImpl.java` al crear una nueva clase Java™ en `src/test/java` en una estructura de carpetas de paquetes Java™ que refleja la ubicación de la clase Java™ que se va a probar.

   ![Cree un nuevo archivo BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Ya que estamos probando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   cree una clase Java™ de prueba unitaria correspondiente en

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   El `Test` sufijo en el archivo de prueba unitaria, `BylineImplTest.java` es una convención, que nos permite

   1. Identifíquelo fácilmente como archivo de prueba _para_ `BylineImpl.java`
   1. Pero también diferenciar el archivo de prueba _de_ la clase que se está sometiendo a prueba, `BylineImpl.java`

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

1. El primer método `public void setUp() { .. }` está anotado con JUnit `@BeforeEach`, que indica al ejecutor de pruebas de JUnit que ejecute este método antes de ejecutar cada método de prueba en esta clase. Esto proporciona un lugar práctico para inicializar el estado de prueba común requerido por todas las pruebas.

1. Los métodos siguientes son los métodos de prueba, cuyos nombres llevan el prefijo `test` por convención, y marcado con la etiqueta `@Test` anotación. Tenga en cuenta que, de forma predeterminada, todas nuestras pruebas están configuradas para fallar, ya que aún no las hemos implementado.

   Para empezar, comenzamos con un único método de prueba para cada método público en la clase que estamos probando, así que:

   | BylineImpl.java |              | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | está probado por | testGetName() |
   | getOccupations() | está probado por | testGetOccupations() |
   | isEmpty() | está probado por | testIsEmpty() |

   Estos métodos se pueden ampliar según sea necesario, como veremos más adelante en este capítulo.

   Cuando se ejecuta esta clase de prueba JUnit (también conocida como Caso de prueba JUnit), cada método se marca con la variable `@Test` se ejecutará como una prueba que puede superar o dar error.

![BylineImplTest generado](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Ejecute el caso de prueba JUnit haciendo clic con el botón derecho en el `BylineImplTest.java` archivo y pulse **Ejecutar**.
Como se esperaba, todas las pruebas fallan, ya que aún no se han implementado.

   ![Ejecutar como prueba conjunta](assets/unit-testing/run-junit-tests.png)

   *Haga clic con el botón derecho en BylineImplTests.java > Ejecutar*

## Revisión de BylineImpl.java {#reviewing-bylineimpl-java}

Al escribir pruebas unitarias, existen dos enfoques principales:

* [Desarrollo impulsado por pruebas o TDD](https://en.wikipedia.org/wiki/Test-driven_development), que implica escribir las pruebas unitarias de forma incremental, inmediatamente antes de desarrollar la implementación; escriba una prueba y escriba la implementación para que la prueba se apruebe.
* Implementación: primero desarrollo, que implica desarrollar primero el código de trabajo y, a continuación, escribir pruebas que validen dicho código.

En este tutorial, se utiliza el último enfoque (ya que ya hemos creado un **BylineImpl.java** en un capítulo anterior). Debido a esto, debemos revisar y comprender los comportamientos de sus métodos públicos, pero también algunos de sus detalles de implementación. AEM Esto puede sonar contrario, ya que una buena prueba solo debe preocuparse por las entradas y salidas, sin embargo, cuando se trabaja en el área de la implementación, hay varias consideraciones de implementación que deben entenderse para construir pruebas de trabajo.

AEM AEM AEM AEM La TDD en el contexto de la requiere un nivel de experiencia y la mejor manera de adoptarla es a través de desarrolladores de competentes en el desarrollo y prueba de unidades de código de la.

## AEM Configuración del contexto de prueba de  {#setting-up-aem-test-context}

AEM AEM AEM La mayoría del código escrito para la se basa en las API de JCR, Sling o, que a su vez requieren el contexto de una aplicación en ejecución para que se ejecute correctamente.

AEM Dado que las pruebas unitarias se ejecutan durante la compilación, no existe ese contexto fuera del contexto de una instancia de ejecución de la. Para facilitar esto, [AEM Mocks de de wcm.io](https://wcm.io/testing/aem-mock/usage.html) crea un contexto ficticio que permite a estas API _mayoritariamente_ AEM actuar como si estuvieran corriendo en la.

1. AEM Creación de un contexto de mediante **wcm.io&#39;s** `AemContext` in **BylineImplTest.java** al agregarla como una extensión JUnit decorada con `@ExtendWith` a la **BylineImplTest.java** archivo. La extensión se encarga de todas las tareas de inicialización y limpieza necesarias. Crear una variable de clase para `AemContext` que se puede utilizar para todos los métodos de prueba.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Esta variable, `ctx`AEM AEM , expone un contexto de ficticio que proporciona algunas abstracciones de Sling y de los recursos de la plataforma de datos:

   * El modelo Sling BylineImpl está registrado en este contexto
   * Las estructuras de contenido JCR simuladas se crean en este contexto
   * Los servicios OSGi personalizados se pueden registrar en este contexto
   * AEM Proporciona varios objetos de prueba y ayudantes comunes necesarios, como objetos SlingHttpServletRequest, varios servicios de Sling y OSGi de prueba, como ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag, etc.
      * *No se han implementado todos los métodos para estos objetos.*
   * Y [mucho más](https://wcm.io/testing/aem-mock/usage.html)!

   El **`ctx`** Este objeto actuará como punto de entrada para la mayoría de nuestros contextos ficticios.

1. En el `setUp(..)` , que se ejecuta antes de cada `@Test` método, defina un estado de prueba de prueba de prueba común:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** AEM registra el modelo de Sling que se va a probar en el contexto de ficticio para que se pueda crear una instancia en `@Test` métodos.
   * **`load().json`** carga estructuras de recursos en el contexto ficticio, lo que permite que el código interactúe con estos recursos como si fueran proporcionados por un repositorio real. Las definiciones de recursos en el archivo **`BylineImplTest.json`** se cargan en el contexto JCR ficticio en **/content**.
   * **`BylineImplTest.json`** aún no existe, así que vamos a crearlo y definir las estructuras de recursos JCR necesarias para la prueba.

1. Los archivos JSON que representan las estructuras de recursos de prueba se almacenan en **core/src/test/resources** siguiendo las mismas rutas de paquete que el archivo de prueba JUnit Java™.

   Cree un archivo JSON en `core/test/resources/com/adobe/aem/guides/wknd/core/models/impl` nombrado **BylineImplTest.json** con el siguiente contenido:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Este JSON define un recurso ficticio (nodo JCR) para la prueba unitaria de componentes Byline. En este punto, el JSON tiene el conjunto mínimo de propiedades necesarias para representar un recurso de contenido de componente de firma, el `jcr:primaryType` y `sling:resourceType`.

   Una regla general al trabajar con pruebas unitarias es crear el conjunto mínimo de contenido, contexto y código ficticio necesario para satisfacer cada prueba. Evite la tentación de crear un contexto ficticio completo antes de escribir las pruebas, ya que a menudo da como resultado artefactos innecesarios.

   Ahora con la existencia de **BylineImplTest.json**, cuando `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` se ejecuta, las definiciones de recursos simuladas se cargan en el contexto en la ruta **/content.**

## Prueba de getName() {#testing-get-name}

Ahora que tenemos una configuración básica de contexto ficticio, vamos a escribir nuestra primera prueba para **getName() de BylineImpl**. Esta prueba debe garantizar el método **getName()** devuelve el nombre creado correctamente almacenado en el recurso &quot;**name&quot;** propiedad.

1. Actualice el **testGetName**(método) en **BylineImplTest.java** como sigue:

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

   * **`String expected`** establece el valor esperado. Estableceremos esto en &quot;**Jane Listo**&quot;.
   * **`ctx.currentResource`** establece el contexto del recurso de prueba con el que se evaluará el código, por lo que se establece en **/content/byline** ya que es allí donde se carga el recurso de contenido de firma ficticia.
   * **`Byline byline`** crea una instancia del modelo Sling de firma adaptándolo desde el objeto de solicitud ficticia.
   * **`String actual`** invoca el método que estamos probando, `getName()`, en el objeto Modelo Sling de firma.
   * **`assertEquals`** afirma que el valor esperado coincide con el valor devuelto por el objeto del modelo Sling de firma. Si estos valores no son iguales, la prueba fallará.

1. Ejecute la prueba... y falla con un `NullPointerException`.

   Esta prueba NO falla porque nunca definimos un `name` en el archivo JSON ficticio, lo que provocará que la prueba falle, aunque la ejecución de la prueba no haya llegado a ese punto. Esta prueba falla debido a un `NullPointerException` en el propio objeto byline.

1. En el `BylineImpl.java`, si `@PostConstruct init()` genera una excepción porque evita que el modelo Sling cree instancias y provoca que el objeto del modelo Sling sea nulo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Resulta que mientras el servicio OSGi de ModelFactory se proporciona a través de la `AemContext` (a través del contexto de Apache Sling), no se implementan todos los métodos, incluidos los siguientes `getModelFromWrappedRequest(...)` que se llama en el de BylineImpl `init()` método. Esto da como resultado una [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), que a término causa `init()` y la consiguiente adaptación de la `ctx.request().adaptTo(Byline.class)` es un objeto nulo.

   Dado que los simulacros proporcionados no pueden acomodar nuestro código, debemos implementar el contexto de simulación nosotros mismos. Para esto, podemos usar Mockito para crear un objeto ModelFactory de simulación, que devuelva un objeto Image de simulación cuando `getModelFromWrappedRequest(...)` se invoca en él.

   Dado que, para crear una instancia del modelo Sling de firma, este contexto ficticio debe estar configurado, podemos agregarlo al `@Before setUp()` método. También es necesario añadir la variable `MockitoExtension.class` a la `@ExtendWith` anotación encima de **BylineImplTest** clase.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca la clase de caso de prueba que se va a ejecutar con el [Extensión de Júpiter JUnit de Mockito](https://www.javadoc.io/static/org.mockito/mockito-junit-jupiter/4.11.0/org/mockito/junit/jupiter/MockitoExtension.html) que permite utilizar las anotaciones @Mock para definir objetos de prueba en el nivel de clase.
   * **`@Mock private Image`** crea un objeto ficticio de tipo `com.adobe.cq.wcm.core.components.models.Image`. Se define en el nivel de clase de modo que, según sea necesario, `@Test` Los métodos de pueden modificar su comportamiento según sea necesario.
   * **`@Mock private ModelFactory`** crea un objeto ficticio de tipo ModelFactory. Esta es una burla de Mockito pura y no tiene métodos implementados. Se define en el nivel de clase de modo que, según sea necesario, `@Test`Los métodos de pueden modificar su comportamiento según sea necesario.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra un comportamiento ficticio para cuando `getModelFromWrappedRequest(..)` se llama en el objeto ModelFactory ficticio. El resultado definido en `thenReturn (..)` es para devolver el objeto de imagen simulada. Este comportamiento solo se invoca cuando: el primer parámetro es igual a `ctx`El objeto request de, el segundo parámetro es cualquier objeto Resource y el tercer parámetro debe ser la clase de imagen de los componentes principales. Aceptamos cualquier Recurso porque a lo largo de nuestras pruebas estamos configurando el `ctx.currentResource(...)` a varios recursos de prueba definidos en la variable **BylineImplTest.json**. Tenga en cuenta que agregamos el **lenient()** estricto, ya que más adelante desearemos anular este comportamiento de ModelFactory.
   * **`ctx.registerService(..)`.** registra el objeto ModelFactory ficticio en AemContext, con la clasificación de servicio más alta. Esto es necesario, ya que ModelFactory se utiliza en el `init()` se inyecta mediante el `@OSGiService ModelFactory model` field. Para que AemContext se inyecte **nuestra** objeto ficticio, que administra las llamadas a `getModelFromWrappedRequest(..)`, debemos registrarlo como el Servicio de mayor rango de ese tipo (ModelFactory).

1. Vuelva a ejecutar la prueba y, de nuevo, fallará, pero esta vez el mensaje aclara por qué ha fallado.

   ![afirmación de error de nombre de prueba](assets/unit-testing/testgetname-failure-assertion.png)

   *Error de testGetName() debido a una afirmación*

   Recibimos un **AssertionError** lo que significa que la condición assert de la prueba ha fallado y nos dice la **el valor esperado es &quot;Jane Doe&quot;** pero el **el valor real es nulo**. Esto tiene sentido porque &quot;**name&quot;** La propiedad no se ha agregado para burlarse de **/content/byline** definición de recurso en **BylineImplTest.json**, así que vamos a añadirlo:

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

1. Vuelva a ejecutar la prueba y **`testGetName()`** ¡ahora pasa!

   ![aprobación de nombre de prueba](assets/unit-testing/testgetname-pass.png)


## Prueba de getOccupations() {#testing-get-occupations}

¡Muy bien! ¡La primera prueba ha pasado! Vamos a seguir adelante y a probar `getOccupations()`. Dado que la inicialización del contexto ficticio se realizó en el `@Before setUp()`método, está disponible para todos los `@Test` métodos en este caso de prueba, incluidos `getOccupations()`.

Recuerde que este método debe devolver una lista ordenada alfabéticamente de ocupaciones (descendentes) almacenadas en la propiedad de ocupaciones.

1. Actualizar **`testGetOccupations()`** como sigue:

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

   * **`List<String> expected`** defina el resultado esperado.
   * **`ctx.currentResource`** establece el recurso actual para evaluar el contexto con respecto a la definición del recurso de prueba en /content/byline. Esto garantiza que **BylineImpl.java** se ejecuta en el contexto de nuestro recurso ficticio.
   * **`ctx.request().adaptTo(Byline.class)`** crea una instancia del modelo Sling de firma adaptándolo desde el objeto de solicitud ficticia.
   * **`byline.getOccupations()`** invoca el método que estamos probando, `getOccupations()`, en el objeto Modelo Sling de firma.
   * **`assertEquals(expected, actual)`** afirma que la lista esperada es la misma que la lista real.

1. Recuerda, igual que **`getName()`** arriba, el **BylineImplTest.json** no define ocupaciones, por lo que esta prueba fallará si la ejecutamos, ya que `byline.getOccupations()` devolverá una lista vacía.

   Actualizar **BylineImplTest.json** para incluir una lista de ocupaciones, y se establecen en orden no alfabético para garantizar que nuestras pruebas validen que las ocupaciones se ordenan alfabéticamente por **`getOccupations()`**.

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

   ![Obtener pase de Ocupaciones](assets/unit-testing/testgetoccupations-pass.png)

   *testGetOccupations() supera*

## Prueba isEmpty() {#testing-is-empty}

El último método para probar **`isEmpty()`**.

Pruebas `isEmpty()` es interesante, ya que requiere pruebas para varias condiciones. Revisión **BylineImpl.java** de `isEmpty()` método se deben comprobar las condiciones siguientes:

* Devolver verdadero cuando el nombre está vacío
* Devolver verdadero cuando las ocupaciones son nulas o están vacías
* Devolver verdadero cuando la imagen es nula o no tiene URL de origen
* Devuelve &quot;false&quot; cuando el nombre, las ocupaciones y la imagen (con una URL src) están presentes

Para ello, es necesario crear métodos de prueba, cada prueba de una condición específica y nuevas estructuras de recursos simuladas en `BylineImplTest.json` para realizar estas pruebas.

Esta comprobación nos permitió omitir la prueba de cuándo `getName()`, `getOccupations()` y `getImage()` están vacías, ya que el comportamiento esperado de ese estado se prueba mediante `isEmpty()`.

1. La primera prueba probará la condición de un componente completamente nuevo que no tiene propiedades establecidas.

   Añadir una nueva definición de recurso a `BylineImplTest.json`, dándole el nombre semántico &quot;**vaciar**&quot;

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

   **`"empty": {...}`** defina una nueva definición de recurso con el nombre &quot;vacío&quot; que solo tenga un `jcr:primaryType` y `sling:resourceType`.

   Recuerde que cargamos `BylineImplTest.json` en `ctx` antes de la ejecución de cada método de prueba en `@setUp`, por lo que esta nueva definición de recurso está disponible inmediatamente para su uso en pruebas en **/content/empty.**

1. Actualizar `testIsEmpty()` como se indica a continuación, se establece el recurso actual en el nuevo &quot;**vaciar**&quot; definición de recurso ficticio.

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

   Para cada prueba, se utiliza una definición de recurso simulada discreta, actualizar **BylineImplTest.json** con las definiciones de recursos adicionales para **sin nombre** y **sin ocupaciones**.

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

   **`testIsEmpty()`** comprueba la definición de recurso ficticio vacía y afirma que `isEmpty()` es verdadero.

   **`testIsEmpty_WithoutName()`** prueba con una definición de recurso ficticia que tiene ocupaciones pero no tiene nombre.

   **`testIsEmpty_WithoutOccupations()`** prueba con una definición de recurso ficticia que tiene un nombre pero ninguna ocupación.

   **`testIsEmpty_WithoutImage()`** prueba una definición de recurso ficticio con un nombre y ocupaciones, pero establece la imagen ficticia en null. Tenga en cuenta que queremos anular la variable `modelFactory.getModelFromWrappedRequest(..)`comportamiento definido en `setUp()` para asegurarse de que el objeto Image devuelto por esta llamada sea nulo. La función de código auxiliar de Mockito es estricta y no desea código duplicado. Por lo tanto, establecemos la simulación con **`lenient`** configuración para indicar explícitamente que anulamos el comportamiento en la `setUp()` método.

   **`testIsEmpty_WithoutImageSrc()`** prueba una definición de recurso ficticia con un nombre y ocupaciones, pero establece la imagen ficticia para que devuelva una cadena en blanco cuando `getSrc()` se invoca a.

1. Por último, escriba una prueba para asegurarse de que **isEmpty()** devuelve el valor &quot;false&quot; cuando el componente está configurado correctamente. Para esta condición, podemos reutilizar **/content/byline** que representa un componente Byline completamente configurado.

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

![Todas las pruebas se superan](./assets/unit-testing/all-tests-pass.png)

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

Ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama Git `tutorial/unit-testing-solution`.
