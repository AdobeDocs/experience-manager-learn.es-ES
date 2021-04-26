---
title: Prueba de unidad
description: Este tutorial trata la implementación de una prueba de unidad que valida el comportamiento del modelo Sling del componente Byline, creado en el tutorial Componente personalizado .
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
feature: API, AEM tipo de archivo del proyecto
topic: Gestión de contenido, desarrollo
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: e8c36a85bc47adbf76e614f245c3f47d7a50826e
workflow-type: tm+mt
source-wordcount: '3016'
ht-degree: 0%

---


# Prueba de unidad {#unit-testing}

Este tutorial trata la implementación de una prueba de unidad que valida el comportamiento del modelo Sling del componente Byline, creado en el tutorial [Componente personalizado](./custom-component.md).

## Requisitos previos {#prerequisites}

Revise las herramientas e instrucciones necesarias para configurar un [entorno de desarrollo local](overview.md#local-dev-environment).

_Si Java 8 y Java 11 están instalados en el sistema, el ejecutor de pruebas de código VS puede elegir el tiempo de ejecución de Java más bajo al ejecutar las pruebas, lo que provoca errores de prueba. Si esto ocurre, desinstale Java 8._

### Proyecto de inicio

>[!NOTE]
>
> Si ha completado correctamente el capítulo anterior, puede volver a utilizar el proyecto y omitir los pasos para extraer el proyecto de inicio.

Consulte el código de línea base sobre el que se basa el tutorial:

1. Consulte la rama `tutorial/unit-testing-start` de [GitHub](https://github.com/adobe/aem-guides-wknd)

   ```shell
   $ cd aem-guides-wknd
   $ git checkout tutorial/unit-testing-start
   ```

1. Implemente código base en una instancia de AEM local con sus habilidades con Maven:

   ```shell
   $ mvn clean install -PautoInstallSinglePackage
   ```

   >[!NOTE]
   >
   > Si utiliza AEM 6.5 o 6.4, anexe el perfil `classic` a cualquier comando Maven.

   ```shell
   $ mvn clean install -PautoInstallSinglePackage -Pclassic
   ```

Siempre puede ver el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd/tree/tutorial/unit-testing-start) o extraer el código localmente cambiando a la rama `tutorial/unit-testing-start`.

## Objetivo

1. Comprender los conceptos básicos de la prueba de unidades.
1. Obtenga información sobre los marcos y las herramientas que se utilizan habitualmente para probar AEM código.
1. Comprender las opciones para burlarse o simular recursos de AEM al escribir pruebas unitarias.

## Fondo {#unit-testing-background}

En este tutorial, exploraremos cómo escribir [Unit Tests](https://en.wikipedia.org/wiki/Unit_testing) para el [Sling Model](https://sling.apache.org/documentation/bundles/models.html) de nuestro componente Byline (creado en [Creating a custom AEM Component](custom-component.md)). Las pruebas unitarias son pruebas en tiempo de compilación escritas en Java que verifican el comportamiento esperado del código Java. Cada prueba unitaria suele ser pequeña y valida la salida de un método (o unidades de trabajo) frente a los resultados esperados.

Utilizaremos AEM prácticas recomendadas y usaremos:

* [JUnit 5](https://junit.org/junit5/)
* [Marco de pruebas de Mockito](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/)  (que se basa en  [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

## Prueba de unidades y Administrador de nube de Adobe {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud ](https://docs.adobe.com/content/help/es-ES/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) Manager integra la ejecución de pruebas unitarias y los informes de cobertura de  [ ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) código en su canalización CI/CD para ayudar a fomentar y promover la mejor práctica de prueba de unidades AEM código.

Aunque el código de prueba de unidades es una buena práctica para cualquier base de código, al utilizar Cloud Manager es importante aprovechar sus instalaciones de pruebas y creación de informes de calidad de código al proporcionar pruebas de unidades para que Cloud Manager se ejecute.

## Inspect: probar las dependencias de Maven {#inspect-the-test-maven-dependencies}

El primer paso es inspeccionar las dependencias de Maven para admitir la escritura y ejecución de las pruebas. Se requieren cuatro dependencias:

1. JUnit5
1. Marco de pruebas de Mockito
1. Mocks de Apache Sling
1. AEM Mocks Test Framework (por io.wcm)

Las dependencias de prueba **JUnit5**, **Mockito** y **AEM Mocks** se añaden automáticamente al proyecto durante la configuración mediante el [tipo de archivo AEM Maven](project-setup.md).

1. Para ver estas dependencias, abra el POM del reactor principal en **aem-guides-wknd/pom.xml**, vaya a `<dependencies>..</dependencies>` y asegúrese de que las siguientes dependencias estén definidas:

   ```xml
   <dependencies>
       ...       
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.6.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>3.3.3</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>3.0.2</version>
           <scope>test</scope>
       </dependency>        
       ...
   </dependencies>
   ```

1. Abra **aem-guides-wknd/core/pom.xml** y vea que las dependencias de prueba correspondientes están disponibles:

   ```xml
   ...
   <!-- Testing -->
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
       <exclusions>
           <exclusion>
               <groupId>org.apache.sling</groupId>
               <artifactId>org.apache.sling.models.impl</artifactId>
           </exclusion>
           <exclusion>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-simple</artifactId>
           </exclusion>
       </exclusions>
       <scope>test</scope>
   </dependency>
   <!-- Required to be able to support injection with @Self and @Via -->
   <dependency>
       <groupId>org.apache.sling</groupId>
       <artifactId>org.apache.sling.models.impl</artifactId>
       <version>1.4.4</version>
       <scope>test</scope>
   </dependency>
   ...
   ```

   Una carpeta de origen paralela en el proyecto **core** contendrá las pruebas unitarias y los archivos de prueba auxiliares. Esta carpeta **test** proporciona una separación de las clases de prueba del código fuente, pero permite que las pruebas actúen como si estuvieran en los mismos paquetes que el código fuente.

## Creación de la prueba JUnit {#creating-the-junit-test}

Las pruebas unitarias suelen asignar de 1 a 1 con clases Java. En este capítulo, escribiremos una prueba JUnit para el **BylineImpl.java**, que es el modelo Sling que respalda el componente Byline.

![Carpeta src de prueba unitaria](assets/unit-testing/core-src-test-folder.png)

*Ubicación en la que se almacenan las pruebas unitarias.*

1. Cree una prueba de unidad para `BylineImpl.java` haciendo una nueva clase Java en `src/test/java` en una estructura de carpetas de paquetes Java que refleje la ubicación de la clase Java que se va a probar.

   ![Crear un nuevo archivo BylineImplTest.java](assets/unit-testing/new-bylineimpltest.png)

   Ya que estamos probando

   * `src/main/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImpl.java`

   cree la clase Java de prueba unitaria correspondiente en

   * `src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`

   El sufijo `Test` del archivo de prueba de unidad, `BylineImplTest.java` es una convención que nos permite

   1. Identifíquelo fácilmente como el archivo de prueba _para_ `BylineImpl.java`
   1. Pero también, diferencie el archivo de prueba _de_ de la clase que se está probando, `BylineImpl.java`



## Revisión de BylineImplTest.java {#reviewing-bylineimpltest-java}

En este punto, el archivo de prueba JUnit es una clase Java vacía. Actualice el archivo con el siguiente código:

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

1. El primer método `public void setUp() { .. }` se anota con el `@BeforeEach` de JUnit, que ordena al ejecutor de pruebas JUnit que ejecute este método antes de ejecutar cada método de prueba en esta clase. Esto proporciona un lugar práctico para inicializar el estado de prueba común requerido por todas las pruebas.

2. Los métodos siguientes son los métodos de prueba, cuyos nombres llevan el prefijo `test` por convención y están marcados con la anotación `@Test`. Tenga en cuenta que, de forma predeterminada, todas nuestras pruebas están configuradas para fallar, ya que aún no las hemos implementado.

   Para empezar, empezamos con un único método de prueba para cada método público en la clase que estamos probando, así que:

   | BylineImpl.java |  | BylineImplTest.java |
   | ------------------|--------------|---------------------|
   | getName() | es probado por | testGetName() |
   | getOccupations() | es probado por | testGetOccupations() |
   | isEmpty() | es probado por | testIsEmpty() |

   Estos métodos se pueden ampliar según sea necesario, como veremos más adelante en este capítulo.

   Cuando se ejecuta esta clase de prueba JUnit (también conocida como JUnit Test Case), cada método marcado con `@Test` se ejecutará como una prueba que puede pasar o fallar.

![BylineImplTest generado](assets/unit-testing/bylineimpltest-stub-methods.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Ejecute el JUnit Test Case haciendo clic con el botón derecho en el archivo `BylineImplTest.java` y pulsando **Run**.
Como se esperaba, todas las pruebas fallan, ya que aún no se han implementado.

   ![Ejecutar como prueba de junte](assets/unit-testing/run-junit-tests.png)

   *Haga clic con el botón derecho en BylineImplTests.java > Ejecutar*

## Revisión de BylineImpl.java {#reviewing-bylineimpl-java}

Al escribir pruebas unitarias, hay dos enfoques principales:

* [Desarrollo](https://en.wikipedia.org/wiki/Test-driven_development) impulsado por TDD o pruebas, que implica la escritura de las pruebas unitarias de manera incremental, inmediatamente antes de que se desarrolle la implementación; escriba una prueba, escriba la implementación para que se supere.
* Implementación: primer desarrollo, que implica primero desarrollar código de trabajo y, después, escribir pruebas que validen dicho código.

En este tutorial, se utiliza el último método (ya que ya hemos creado un **BylineImpl.java** en un capítulo anterior). Debido a esto, debemos revisar y entender el comportamiento de sus métodos públicos, pero también algunos de sus detalles de implementación. Esto puede sonar contrario, ya que una buena prueba sólo debería ocuparse de los insumos y los productos, pero al trabajar en AEM, hay una variedad de consideraciones de aplicación que es necesario entender para construir ensayos de trabajo.

En el contexto de la AEM, el TDD requiere un nivel de experiencia y es mejor que lo adopten los desarrolladores AEM que son expertos en AEM desarrollo y pruebas unitarias de AEM código.

## Configuración AEM contexto de prueba {#setting-up-aem-test-context}

La mayoría del código escrito para AEM se basa en las API de JCR, Sling o AEM, que a su vez requieren que el contexto de una AEM en ejecución se ejecute correctamente.

Dado que las pruebas unitarias se ejecutan en la compilación, fuera del contexto de una instancia de AEM en ejecución, no existe dicho contexto. Para facilitarle este proceso, [AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) de wcm.io crea un contexto de prueba que permite que estas API _principalmente_ actúen como si se estuvieran ejecutando en AEM.

1. Cree un contexto AEM utilizando **wcm.io&#39;s** `AemContext` en **BylineImplTest.java** añadiéndolo como una extensión JUnit decorada con `@ExtendWith` en el archivo **BylineImplTest.java**. La extensión se encarga de todas las tareas de inicialización y limpieza necesarias. Cree una variable de clase para `AemContext` que se pueda usar para todos los métodos de prueba.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Esta variable, `ctx`, expone un contexto de AEM simulación que proporciona una serie de abstracciones de AEM y Sling:

   * El modelo de Sling de BylineImpl se registrará en este contexto
   * Se crean estructuras de contenido JCR simuladas en este contexto
   * Los servicios personalizados de OSGi se pueden registrar en este contexto
   * Proporciona una variedad de objetos de prueba y elementos de ayuda requeridos, como objetos SlingHttpServletRequest , una variedad de servicios de Sling y OSGi AEM como ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, Tag, etc.
      * *Tenga en cuenta que no todos los métodos para estos objetos están implementados.*
   * Y [mucho más](https://wcm.io/testing/aem-mock/usage.html)!

   El objeto **`ctx`** actuará como punto de entrada para la mayor parte de nuestro contexto de prueba.

1. En el método `setUp(..)` , que se ejecuta antes de cada método `@Test` , defina un estado de prueba burda común:

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** registra el modelo de Sling que se va a probar, en el contexto de AEM simulación, para que se pueda crear una instancia de él en los  `@Test` métodos .
   * **`load().json`** carga estructuras de recursos en el contexto de prueba, lo que permite que el código interactúe con estos recursos como si los hubiera proporcionado un repositorio real. Las definiciones de recursos del archivo **`BylineImplTest.json`** se cargan en el contexto JCR de prueba en **/content**.
   * **`BylineImplTest.json`** todavía no existe, por lo que vamos a crearlo y definir las estructuras de recursos JCR que son necesarias para la prueba.

1. Los archivos JSON que representan las estructuras de recursos de prueba se almacenan en **core/src/test/resources** siguiendo las mismas rutas de paquetes que el archivo de prueba Java de JUnit.

   Cree un nuevo archivo JSON en **core/test/resources/com/adobe/aem/guides/wknd/core/models/impl** llamado **BylineImplTest.json** con el siguiente contenido:

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Este JSON define un recurso de prueba (nodo JCR) para la prueba de unidad de componente Byline. En este punto, el JSON tiene el conjunto mínimo de propiedades necesarias para representar un recurso de contenido de componente Byline, `jcr:primaryType` y `sling:resourceType`.

   Una regla general al trabajar con pruebas unitarias es crear el conjunto mínimo de contenido falso, contexto y código necesario para satisfacer cada prueba. Evite la tentación de crear un contexto de burla completo antes de escribir las pruebas, ya que a menudo resulta en artefactos innecesarios.

   Ahora con la existencia de **BylineImplTest.json**, cuando se ejecuta `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")`, las definiciones de recursos de prueba se cargan en el contexto en la ruta **/content.**

## Probando getName() {#testing-get-name}

Ahora que tenemos una configuración básica de contexto de prueba, vamos a escribir nuestra primera prueba para **getName()** de BylineImpl. Esta prueba debe garantizar que el método **getName()** devuelve el nombre de autor correcto almacenado en la propiedad &quot;**name&quot;** del recurso.

1. Actualice el método **testGetName**() en **BylineImplTest.java** de la siguiente manera:

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
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
   * **`ctx.currentResource`** establece el contexto del recurso de prueba con el que evaluar el código, por lo que se establece en  **/content/** bylineas, ya que allí se carga el recurso de contenido de firma simulado.
   * **`Byline byline`** crea una instancia del modelo de Sling de firma adaptándolo al objeto de solicitud de prueba.
   * **`String actual`** invoca el método que estamos probando,  `getName()`, en el objeto Modelo Sling de firma.
   * **`assertEquals`** afirma que el valor esperado coincide con el valor devuelto por el objeto de firma del Modelo Sling. Si estos valores no son iguales, la prueba fallará.

1. Ejecute la prueba... y falla con un `NullPointerException`.

   Tenga en cuenta que esta prueba NO falla porque nunca hemos definido una propiedad `name` en el JSON de prueba, lo que provocará que la prueba falle. Sin embargo, la ejecución de la prueba no ha llegado a ese punto. Esta prueba falla debido a un `NullPointerException` en el propio objeto de firma.

1. En `BylineImpl.java`, si `@PostConstruct init()` lanza una excepción, impide que el Modelo Sling cree una instancia y hace que el objeto del Modelo Sling sea nulo.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Resulta que aunque el servicio ModelFactory OSGi se proporciona a través de `AemContext` (a través del contexto Apache Sling), no todos los métodos se implementan, incluido `getModelFromWrappedRequest(...)` que se llama en el método `init()` de BylineImpl. Esto resulta en un [AbstractMethodError](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/AbstractMethodError.html), que en el término hace que `init()` falle y la adaptación resultante del `ctx.request().adaptTo(Byline.class)` es un objeto nulo.

   Dado que los burlones proporcionados no pueden acomodar nuestro código, debemos implementar el contexto de burla nosotros mismos. Para esto, podemos usar Mockito para crear un objeto ModelFactory falso, que devuelve un objeto de imagen de broma cuando se invoca `getModelFromWrappedRequest(...)` en él.

   Dado que para crear incluso una instancia del modelo de Sling de firma, este contexto de simulación debe estar en su lugar, podemos agregarlo al método `@Before setUp()` . También es necesario agregar `MockitoExtension.class` a la anotación `@ExtendWith` por encima de la clase **BylineImplTest**.

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

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marca la clase Test Case para que se ejecute con la extensión  [Mockito JUnit Jupiter ](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) que permite el uso de las anotaciones @Mock para definir objetos simulados a nivel de clase.
   * **`@Mock private Image`** crea un objeto de simulación de tipo  `com.adobe.cq.wcm.core.components.models.Image`. Tenga en cuenta que esto se define a nivel de clase para que, según sea necesario, los métodos `@Test` puedan modificar su comportamiento según sea necesario.
   * **`@Mock private ModelFactory`** crea un objeto de simulación de tipo ModelFactory. Tenga en cuenta que esta es una burla pura de Mockito y no tiene métodos implementados en ella. Tenga en cuenta que esto se define a nivel de clase para que, según sea necesario, los métodos `@Test`puedan modificar su comportamiento según sea necesario.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** registra el comportamiento de prueba para cuándo  `getModelFromWrappedRequest(..)` se llama en el objeto ModelFactory de prueba. El resultado definido en `thenReturn (..)` es devolver el objeto de imagen de prueba. Tenga en cuenta que este comportamiento solo se invoca cuando: el primer parámetro es igual al objeto de solicitud de `ctx`, el segundo parámetro es cualquier objeto Resource y el tercer parámetro debe ser la clase Core Components Image. Aceptamos cualquier recurso porque a lo largo de nuestras pruebas estableceremos el `ctx.currentResource(...)` en varios recursos de prueba definidos en el **BylineImplTest.json**. Tenga en cuenta que agregamos la estricta **lenient()** porque más adelante querremos anular este comportamiento de ModelFactory.
   * **`ctx.registerService(..)`.** registra el objeto ModelFactory falso en AemContext, con la clasificación de servicio más alta. Esto es necesario, ya que ModelFactory utilizado en el `init()` de BylineImpl se inserta mediante el campo `@OSGiService ModelFactory model`. Para que AemContext inserte **nuestro** objeto de prueba, que gestiona las llamadas a `getModelFromWrappedRequest(..)`, debemos registrarlo como el servicio de mayor rango de ese tipo (ModelFactory).

1. Vuelva a ejecutar la prueba y, de nuevo, falla, pero esta vez el mensaje es claro por qué ha fallado.

   ![afirmación de error en el nombre de la prueba](assets/unit-testing/testgetname-failure-assertion.png)

   *error testGetName() debido a una afirmación*

   Recibimos un **AssertionError** que significa que la condición de aserción en la prueba falló y nos dice que el **valor esperado es &quot;Jane Doe&quot;** pero el **valor real es nulo**. Esto tiene sentido porque la propiedad &quot;**name&quot;** no se ha agregado para burlarse de la definición de recurso **/content/byline** en **BylineImplTest.json**, así que vamos a agregarla:

1. Actualice **BylineImplTest.json** para definir `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Vuelva a ejecutar la prueba y **`testGetName()`** ahora pasa.

   ![pase de nombre de la prueba](assets/unit-testing/testgetname-pass.png)


## Prueba de getOccupations() {#testing-get-occupations}

¡Ok, bueno! ¡Nuestra primera prueba ha pasado! Avancemos y probemos `getOccupations()`. Dado que la inicialización del contexto de prueba se realiza en el método `@Before setUp()`, estará disponible para todos los métodos `@Test` en este caso de prueba, incluido `getOccupations()`.

Recuerde que este método debe devolver una lista ordenada alfabéticamente de ocupaciones (descendente) almacenadas en la propiedad occupations .

1. Actualice **`testGetOccupations()`** como se indica a continuación:

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
   * **`ctx.currentResource`** establece el recurso actual para evaluar el contexto en la definición de recurso de prueba en /content/byline. Esto garantiza que **BylineImpl.java** se ejecute en el contexto de nuestro recurso de prueba.
   * **`ctx.request().adaptTo(Byline.class)`** crea una instancia del modelo de Sling de firma adaptándolo al objeto de solicitud de prueba.
   * **`byline.getOccupations()`** invoca el método que estamos probando,  `getOccupations()`, en el objeto Modelo Sling de firma.
   * **`assertEquals(expected, actual)`** afirma que la lista esperada es la misma que la lista real.

1. Recuerde, al igual que **`getName()`** anterior, **BylineImplTest.json** no define ocupaciones, por lo que esta prueba fallará si la ejecutamos, ya que `byline.getOccupations()` devolverá una lista vacía.

   Actualice **BylineImplTest.json** para incluir una lista de ocupaciones, y se configurarán en orden no alfabético para garantizar que nuestras pruebas validen que las ocupaciones se ordenen alfabéticamente por **`getOccupations()`**.

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

1. Ejecute la prueba, y de nuevo pasamos! ¡Parece que conseguir las ocupaciones ordenadas funciona!

   ![Obtener pase de ocupaciones](assets/unit-testing/testgetoccupations-pass.png)

   *pasadas testGetOccupations()*

## Prueba isEmpty() {#testing-is-empty}

El último método para probar **`isEmpty()`**.

La prueba `isEmpty()` es interesante ya que requiere pruebas para una variedad de condiciones. Al revisar el método **BylineImpl.java** `isEmpty()` se deben probar las siguientes condiciones:

* Devuelve true cuando el nombre está vacío
* Devolver verdadero cuando las ocupaciones son nulas o están vacías
* Devolver verdadero cuando la imagen es nula o no tiene dirección URL src
* Devolver falso cuando el nombre, las ocupaciones y la imagen (con una URL src) están presentes

Para ello, es necesario crear nuevos métodos de prueba, cada uno de los cuales pruebe una condición específica, así como nuevas estructuras de recursos de prueba en `BylineImplTest.json` para impulsar estas pruebas.

Tenga en cuenta que esta comprobación nos permitió omitir las pruebas para saber cuándo `getName()`, `getOccupations()` y `getImage()` están vacías, ya que el comportamiento esperado de ese estado se prueba mediante `isEmpty()`.

1. La primera prueba probará la condición de un componente completamente nuevo, que no tiene propiedades definidas.

   Agregue una nueva definición de recurso a `BylineImplTest.json`, dándole el nombre semántico &quot;**empty**&quot;

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

   **`"empty": {...}`** defina una nueva definición de recurso denominada &quot;empty&quot; que solo tenga un  `jcr:primaryType` y  `sling:resourceType`.

   Recuerde que cargamos `BylineImplTest.json` en `ctx` antes de la ejecución de cada método de prueba en `@setUp`, por lo que esta nueva definición de recurso está disponible inmediatamente para nosotros en pruebas en **/content/empty.**

1. Actualice `testIsEmpty()` como se indica a continuación, configurando el recurso actual en la nueva definición de recurso de prueba &quot;**empty**&quot;.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Ejecute la prueba y asegúrese de que pasa.

1. A continuación, cree un conjunto de métodos para asegurarse de que si alguno de los puntos de datos requeridos (nombre, ocupaciones o imagen) está vacío, `isEmpty()` devuelve el valor &quot;True&quot;.

   Para cada prueba se utiliza una definición discreta de recurso de prueba, actualice **BylineImplTest.json** con las definiciones de recursos adicionales para **sin nombre** y **sin ocupaciones**.

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

   **`testIsEmpty()`** prueba con la definición vacía del recurso de prueba y afirma que  `isEmpty()` es verdadera.

   **`testIsEmpty_WithoutName()`** prueba con una definición de recurso de prueba que tiene ocupaciones pero no tiene nombre.

   **`testIsEmpty_WithoutOccupations()`** prueba con una definición de recurso de prueba que tiene un nombre pero no ocupaciones.

   **`testIsEmpty_WithoutImage()`** prueba con una definición de recurso de prueba con un nombre y ocupaciones, pero establece la imagen de prueba para que vuelva a ser nula. Tenga en cuenta que queremos anular el comportamiento `modelFactory.getModelFromWrappedRequest(..)`definido en `setUp()` para garantizar que el objeto Image devuelto por esta llamada sea nulo. La función de código auxiliar de Mockito es estricta y no desea código de código engañoso. Por lo tanto, configuramos la maqueta con la configuración **`lenient`** para tener en cuenta explícitamente que estamos anulando el comportamiento en el método `setUp()`.

   **`testIsEmpty_WithoutImageSrc()`** prueba con una definición de recurso de prueba con un nombre y ocupaciones, pero establece la imagen de prueba para devolver una cadena en blanco cuando  `getSrc()` se invoca.

1. Por último, escriba una prueba para asegurarse de que **isEmpty()** devuelva el valor false cuando el componente esté configurado correctamente. Para esta condición, podemos reutilizar **/content/byline** que representa un componente Byline completamente configurado.

   ```java
   @Test
   public void testIsNotEmpty() {
       ctx.currentResource("/content/byline");
       when(image.getSrc()).thenReturn("/content/bio.png");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertFalse(byline.isEmpty());
   }
   ```

1. Ahora ejecute todas las pruebas unitarias en el archivo BylineImplTest.java y revise el resultado del informe de prueba Java.

![Todas las pruebas pasan](./assets/unit-testing/all-tests-pass.png)

## Ejecución de pruebas unitarias como parte de la compilación {#running-unit-tests-as-part-of-the-build}

Se requieren pruebas unitarias para pasar como parte de la compilación de maven. Esto garantiza que todas las pruebas pasen correctamente antes de que se implemente una aplicación. Al ejecutar los objetivos de Maven, como el paquete o la instalación, se invoca automáticamente y se requiere pasar todas las pruebas unitarias del proyecto.

```shell
$ mvn package
```

![éxito del paquete mvn](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

Del mismo modo, si cambiamos un método de prueba para que falle, la compilación falla e informa de qué prueba ha fallado y por qué.

![error en el paquete mvn](assets/unit-testing/mvn-package-fail.png)

## Revise el código {#review-the-code}

Vea el código terminado en [GitHub](https://github.com/adobe/aem-guides-wknd) o revise e implemente el código localmente en la rama `tutorial/unit-testing-solution` de Git.
