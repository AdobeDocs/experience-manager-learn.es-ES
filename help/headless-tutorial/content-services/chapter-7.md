---
title: Capítulo 7 - Uso de AEM Content Services desde una aplicación móvil - Content Services
description: El capítulo 7 del tutorial ejecuta la aplicación móvil de Android para consumir contenido de creación de AEM Content Services.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '1408'
ht-degree: 0%

---


# Capítulo 7 - Uso de AEM Content Services desde una aplicación móvil

El capítulo 7 del tutorial utiliza una aplicación móvil nativa de Android para consumir contenido de AEM Content Services.

## La aplicación móvil de Android

Este tutorial utiliza una **aplicación móvil nativa simple de Android** para consumir y mostrar contenido de Evento expuesto por AEM Content Services.

El uso de [Android](https://developer.android.com/) no es muy importante, y la aplicación móvil que consume podría escribirse en cualquier marco para cualquier plataforma móvil, por ejemplo iOS.

Android se utiliza para tutoriales debido a la capacidad de ejecutar un emulador de Android en Windows, macOs y Linux, su popularidad y que puede escribirse como Java, un lenguaje que los desarrolladores de AEM entienden bien.

*La aplicación móvil de Android del tutorial ****no tiene por objeto indicar cómo crear aplicaciones de Android Mobile o transmitir las prácticas recomendadas de desarrollo de Android, sino más bien ilustrar cómo se puede utilizar AEM Content Services desde una aplicación móvil.*

### Cómo AEM Content Services impulsa la experiencia de aplicaciones móviles

![Asignación de aplicaciones móviles a servicios de contenido](assets/chapter-7/content-services-mapping.png)

1. El **logotipo** tal como se define en el [!DNL Events API] componente de imagen **de la página**.
1. La **línea de etiqueta** tal como se define en el [!DNL Events API] componente de texto **de la página**.
1. Esta **lista de Evento** se deriva de la serialización de los fragmentos de contenido de Evento, expuestos a través del **componente de Lista de fragmento de contenido** configurado.

## Demostración de aplicaciones móviles

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuración de la aplicación móvil para uso de host no local

Si AEM Publish no se está ejecutando en **http://localhost:4503**, el host y el puerto se pueden actualizar en el [!DNL Settings] de la aplicación móvil para que señalen al puerto/host de AEM Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Ejecución local de la aplicación móvil

1. Descargue e instale el [Android Studio](https://developer.android.com/studio/install) para instalar el emulador de Android.
1. **** Descargar el  [!DNL APK] archivo de Android  [GitHub > Recursos > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abrir **Android Studio**
   * En el lanzamiento inicial de Android Studio, aparecerá un mensaje para instalar el [!DNL Android SDK]. Acepte los valores predeterminados y finalice la instalación.
1. Abra Android Studio y seleccione **Perfil o Depurar APK**
1. Seleccione el archivo APK (**wknd-mobile.x.x.x.apk**) descargado en el paso 2 y haga clic en **Aceptar**
   * Si se le solicita **Crear una nueva carpeta** o **Usar existente**, seleccione **Usar existente**.
1. En el lanzamiento inicial de Android Studio, haga clic con el botón derecho en **wknd-mobile.x.x.x** en la lista Proyectos y seleccione **Open Module Settings**.
   * En **Módulos > wknd-mobile.x.x.x > Ficha Dependencias**, seleccione **Plataforma de la API de Android 29**. Toque Aceptar para cerrar y guardar los cambios.
   * Si no lo hace, verá el error &quot;Seleccione el SDK para Android&quot; al intentar iniciar el emulador.
1. Abra el **Administrador de AVD** seleccionando **Herramientas > Administrador de AVD** o tocando el icono **Administrador de AVD** en la barra superior.
1. En la ventana **Administrador de AVD**, haga clic en **+ Crear dispositivo virtual...** si aún no tiene el dispositivo registrado.
   1. En la parte izquierda, seleccione la categoría **Teléfono**.
   1. Seleccione un **Píxel 2**.
   1. Haga clic en el botón **Siguiente**.
   1. Seleccione **Q** con **Nivel de API 29**.
      * Tras el inicio inicial de AVD Manager, se le pedirá que descargue la API con versiones. Haga clic en el vínculo Descargar junto a la versión &quot;Q&quot; y complete la descarga y la instalación.
   1. Haga clic en el botón **Siguiente**.
   1. Haga clic en el botón **Finalizar**.
1. Cierre la ventana **Administrador de AVD**.
1. En la barra de menús superior, seleccione **wknd-mobile.x.x.x** en la lista desplegable **Ejecutar/Editar configuraciones**.
1. Toque el botón **Ejecutar** junto a la **Ejecutar/Editar configuración** seleccionada
1. En la ventana emergente, seleccione el dispositivo virtual **[!DNL Pixel 2 API 29]** recién creado y toque **Aceptar**
1. Si la aplicación [!DNL WKND Mobile] no se carga inmediatamente, busque y toque el icono **[!DNL WKND]** desde la pantalla de inicio de Android en el emulador.
   * Si el emulador se inicia pero la pantalla del emulador sigue siendo negra, toque el botón **power** en la ventana de herramientas del emulador junto a la ventana del emulador.
   * Para desplazarse dentro del dispositivo virtual, haga clic y mantenga presionada la tecla y arrastre.
   * Para actualizar el contenido desde AEM, tire hacia abajo desde la parte superior hasta el icono Actualizar
se muestra y se libera.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## El código de la aplicación móvil

Esta sección resalta el código de la aplicación móvil de Android que interactúa más y depende de AEM Content Services y de su salida JSON.

Tras la carga, la aplicación móvil hace `HTTP GET` a `/content/wknd-mobile/en/api/events.model.json`, que es el punto final de AEM Content Services configurado para proporcionar el contenido que conduce a la aplicación móvil.

Dado que la plantilla editable de la API de Eventos (`/content/wknd-mobile/en/api/events.model.json`) está bloqueada, la aplicación móvil se puede codificar para buscar información específica en ubicaciones específicas en la respuesta JSON.

### Flujo de código de alto nivel

1. Al abrir la aplicación [!DNL WKND Mobile] se invoca una solicitud `HTTP GET` a AEM Publish en `/content/wknd-mobile/en/api/events.model.json` para recopilar el contenido y rellenar la interfaz de usuario de la aplicación móvil.
2. Tras recibir el contenido de AEM, cada uno de los tres elementos de vista de la aplicación móvil, el logotipo **, la línea de etiquetas y la lista de evento**, se inicializan con el contenido de AEM.
   * Para enlazar el contenido de AEM al elemento de vista de la aplicación móvil, el JSON que representa cada componente de AEM, es un objeto asignado a un POJO de Java, que a su vez está enlazado al elemento de Vista de Android.
      * Componente de imagen JSON → POJO de logotipo → Vista de imagen de logotipo
      * Text Component JSON → TagLine POJO → Text ImageView
      * Lista de fragmentos de contenido JSON → Eventos POJO → Eventos RecyclerView
   * *El código de la aplicación móvil puede asignar el JSON a los POJO debido a las ubicaciones conocidas dentro de la buena respuesta JSON. Recuerde que las claves JSON de &quot;image&quot;, &quot;text&quot; y &quot;contentfragmentlist&quot; están dictadas por los nombres de nodo AEM Components. Si estos nombres de nodo cambian, la aplicación móvil se interrumpirá, ya que no sabrá cómo crear el contenido necesario a partir de los datos de JSON.*

#### Invocación del punto final de los servicios de contenido de AEM

A continuación se muestra una destilación del código en el `MainActivity` de la aplicación móvil, responsable de invocar AEM Content Services para recopilar el contenido que impulsa la experiencia de la aplicación móvil.

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` es el enlace de inicialización para la aplicación móvil y registra los 3 elementos personalizados  `ViewBinders` responsables de analizar el JSON y enlazar los valores a los  `View` elementos.

`initApp(...)` a continuación, se le llama para solicitar la GET HTTP al punto final de los servicios de contenido de AEM en AEM Publish para recopilar el contenido. Tras recibir una respuesta JSON válida, la respuesta JSON se pasa a cada `ViewBinder` que es responsable de analizar el JSON y de enlazarlo a los elementos `View` móviles.

#### Análisis de la respuesta de JSON

Luego veremos el `LogoViewBinder`, que es sencillo, pero destaca varias consideraciones importantes.

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

La primera línea de `bind(...)` desplaza hacia abajo por la respuesta JSON a través de las claves **:items → raíz → :items** que representa el Contenedor Diseño de AEM al que se agregaron los componentes.

Desde aquí se realiza una comprobación para una clave denominada **image**, que representa el componente Imagen (de nuevo, es importante que este nombre de nodo → clave JSON sea estable). Si este objeto existe, lee y asigna al [POJO de imagen personalizado](#image-pojo) mediante la biblioteca Jackson `ObjectMapper`. La imagen POJO se analiza a continuación.

Finalmente, el `src` logotipo se carga en la vista de imagen de Android mediante la biblioteca auxiliar [!DNL Glide].

Tenga en cuenta que debemos proporcionar el esquema de AEM, el host y el puerto (mediante `aemHost`) a la instancia de AEM Publish, ya que AEM Content Services solo proporcionará la ruta de JCR (por ejemplo: `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenido al que se hace referencia.

#### El POJO de imágenes{#image-pojo}

Aunque es opcional, el uso de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) o capacidades similares proporcionadas por otras bibliotecas como Gson, ayuda a asignar estructuras JSON complejas a los POJO de Java sin el tedio de tratar directamente con los propios objetos JSON nativos. En este caso sencillo, asignamos la clave `src` del objeto JSON `image` al atributo `src` del POJO de imagen directamente mediante la anotación `@JSONProperty`.

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

El POJO de Evento, que requiere seleccionar muchos más puntos de datos del objeto JSON, se beneficia de esta técnica más que la imagen simple, que todo lo que queremos es el `src`.

## Explorar la experiencia de la aplicación móvil

Ahora que sabe cómo AEM Content Services puede impulsar la experiencia nativa de Mobile, utilice lo que ha aprendido para realizar los siguientes pasos y ver los cambios reflejados en la aplicación móvil.

Después de cada paso, actualice la aplicación móvil y compruebe la actualización a la experiencia móvil.

1. Crear y publicar **nuevo [!DNL Event] fragmento de contenido**
1. Cancelar la publicación de un **fragmento de contenido existente [!DNL Event]**
1. Publicar una actualización en la **Línea de etiquetas**

## Felicitaciones

**¡Ha completado el tutorial AEM sin cabeza!**

Para obtener más información sobre AEM Content Services y AEM como un CMS sin cabeza, visite la documentación y otros materiales de habilitación del Adobe:

* [Uso de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guía del usuario de componentes principales de AEM WCM](https://docs.adobe.com/content/help/es-ES/experience-manager-core-components/using/introduction.html)
* [Biblioteca de componentes principales de AEM WCM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM proyecto GitHub de componentes principales de WCM](https://github.com/adobe/aem-core-wcm-components)
* [Componentes principales de AEM WCM: consulte al experto](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Muestra de código del exportador de componentes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
