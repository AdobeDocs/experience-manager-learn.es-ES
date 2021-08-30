---
title: Capítulo 7 - Consumo de servicios de contenido AEM desde una aplicación móvil - Content Services
description: El capítulo 7 del tutorial ejecuta la aplicación móvil de Android para consumir contenido creado desde AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
source-git-commit: ac93d6ba636e64ba6d8bbdb0840810b8f47a25c8
workflow-type: tm+mt
source-wordcount: '1406'
ht-degree: 0%

---


# Capítulo 7 - Consumo de AEM Content Services desde una aplicación móvil

El capítulo 7 del tutorial utiliza una aplicación móvil nativa de Android para consumir contenido de AEM Content Services.

## La aplicación móvil de Android

Este tutorial utiliza una **aplicación móvil nativa simple de Android** para consumir y mostrar el contenido de Evento expuesto por AEM Content Services.

El uso de [Android](https://developer.android.com/) no es muy importante, y la consumidora aplicación móvil podría escribirse en cualquier marco para cualquier plataforma móvil, por ejemplo iOS.

Android se utiliza para tutoriales debido a la capacidad de ejecutar un emulador de Android en Windows, macOs y Linux, su popularidad y que puede escribirse como Java, un idioma que los desarrolladores AEM entienden bien.

*La aplicación móvil de Android del tutorial ****no pretende instruir sobre cómo crear aplicaciones móviles de Android o transmitir las prácticas recomendadas de desarrollo de Android, sino más bien ilustrar cómo se puede consumir AEM servicios de contenido desde una aplicación móvil.*

### Cómo AEM Content Services impulsa la experiencia de la aplicación móvil

![Asignación de aplicaciones móviles a servicios de contenido](assets/chapter-7/content-services-mapping.png)

1. El **logotipo** tal como se define en el [!DNL Events API] **Componente de imagen** de la página.
1. La **línea de etiqueta** tal como se define en el [!DNL Events API] componente de texto **de la página**.
1. Esta **lista de eventos** se deriva de la serialización de los fragmentos de contenido de evento, expuestos a través del **componente de lista de fragmentos de contenido** configurado.

## Demostración de la aplicación móvil

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuración de la aplicación móvil para uso de host no local

Si AEM Publish no se está ejecutando en **http://localhost:4503** el host y el puerto se pueden actualizar en el [!DNL Settings] de la aplicación móvil para que apunten a la propiedad host/puerto de AEM Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Ejecución local de la aplicación móvil

1. Descargue e instale [Android Studio](https://developer.android.com/studio/install) para instalar el emulador de Android.
1. **** Descargue el  [!DNL APK] archivo Android  [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abra **Android Studio**
   * Al iniciar Android Studio por primera vez, aparecerá un mensaje para instalar el [!DNL Android SDK]. Acepte los valores predeterminados y finalice la instalación.
1. Abra Android Studio y seleccione **Perfil o Depuración de APK**
1. Seleccione el archivo APK (**wknd-mobile.x.x.x.apk**) descargado en el paso 2 y haga clic en **OK**
   * Si se le solicita **Crear una nueva carpeta** o **Usar existente**, seleccione **Usar existente**.
1. En el primer inicio de Android Studio, haga clic con el botón derecho en **wknd-mobile.x.x.x** en la lista Proyectos y seleccione **Abrir configuración del módulo**.
   * En **Modules > wknd-mobile.x.x.x > pestaña Dependencies**, seleccione **Android API 29 Platform**. Pulse Aceptar para cerrar y guardar los cambios.
   * Si no lo hace, verá el error &quot;Seleccione el SDK de Android&quot; al intentar iniciar el emulador.
1. Abra el **Administrador de AVD** seleccionando **Herramientas > Administrador de AVD** o tocando el icono **Administrador de AVD** en la barra superior.
1. En la ventana **AVD Manager**, haga clic en **+ Crear dispositivo virtual...** si aún no está registrado en el dispositivo.
   1. En la parte izquierda, seleccione la categoría **Phone** .
   1. Seleccione un **Píxel 2**.
   1. Haga clic en el botón **Next**.
   1. Seleccione **Q** con **Nivel de API 29**.
      * Tras el primer inicio de AVD Manager, se le pedirá que descargue la API con versiones. Haga clic en el vínculo Descargar junto a la versión &quot;Q&quot; y complete la descarga y la instalación.
   1. Haga clic en el botón **Next**.
   1. Haga clic en el botón **Finish**.
1. Cierre la ventana **AVD Manager**.
1. En la barra de menú superior, seleccione **wknd-mobile.x.x.x** en la lista desplegable **Ejecutar/Editar configuraciones**.
1. Pulse el botón **Ejecutar** situado junto al **Ejecutar/Editar configuración** seleccionado
1. En la ventana emergente, seleccione el dispositivo virtual **[!DNL Pixel 2 API 29]** recién creado y pulse **OK**
1. Si la aplicación [!DNL WKND Mobile] no se carga inmediatamente, busque y pulse el icono **[!DNL WKND]** en la pantalla principal de Android en el emulador.
   * Si el emulador se inicia pero la pantalla del emulador sigue siendo negra, pulse el botón **power** en la ventana de herramientas del emulador junto a la ventana del emulador.
   * Para desplazarse dentro del dispositivo virtual, mantenga presionada la tecla y haga clic y arrastre.
   * Para actualizar el contenido desde AEM, tire hacia abajo desde la parte superior hasta el icono Actualizar
y versión.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## El código de la aplicación móvil

Esta sección resalta el código de la aplicación móvil de Android que interactúa más y depende de AEM Content Services y de su salida JSON.

Al cargarse, la aplicación móvil realiza `HTTP GET` a `/content/wknd-mobile/en/api/events.model.json`, que es el punto final de AEM Content Services configurado para proporcionar el contenido que debe conducir a la aplicación móvil.

Como la plantilla editable de la API de eventos (`/content/wknd-mobile/en/api/events.model.json`) está bloqueada, la aplicación móvil se puede codificar para buscar información específica en ubicaciones específicas en la respuesta JSON.

### Flujo de código de alto nivel

1. Al abrir la aplicación [!DNL WKND Mobile] se invoca una solicitud `HTTP GET` a AEM Publish en `/content/wknd-mobile/en/api/events.model.json` para recopilar el contenido que rellenará la interfaz de usuario de la aplicación móvil.
2. Al recibir el contenido de AEM, cada uno de los tres elementos de vista de la aplicación móvil, el logotipo **, la línea de etiqueta y la lista de eventos**, se inicializan con el contenido de AEM.
   * Para enlazar el contenido de AEM al elemento de vista de la aplicación móvil, el JSON que representa cada componente de AEM, es un objeto asignado a un POJO de Java, que a su vez está enlazado al elemento Vista de Android.
      * Componente de imagen JSON → Logotipo POJO → Logo ImageView
      * Componente de texto JSON → POJO TagLine → Texto ImageView
      * Lista de fragmentos de contenido JSON → Eventos POJO → Eventos RecyclerView
   * *El código de la aplicación móvil puede asignar el JSON a los POJO debido a las ubicaciones conocidas dentro de la buena respuesta JSON. Recuerde que las claves JSON de &quot;imagen&quot;, &quot;texto&quot; y &quot;contentfragmentlist&quot; están dictadas por los nombres de nodo de los componentes AEM respaldo. Si cambian estos nombres de nodo, la aplicación móvil se romperá porque no sabrá cómo crear el contenido necesario a partir de los datos JSON.*

#### Invocación del punto final de los servicios de contenido de AEM

A continuación se muestra una destilación del código en el `MainActivity` de la aplicación móvil responsable de invocar AEM Content Services para recopilar el contenido que impulsa la experiencia de la aplicación móvil.

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

`onCreate(..)` es el vínculo de inicialización de la aplicación móvil y registra los 3 elementos personalizados  `ViewBinders` responsables de analizar el JSON y enlazar los valores a los  `View` elementos.

`initApp(...)` a continuación, se llama a , lo que hace que la solicitud de GET HTTP se dirija al punto final de los servicios de contenido de AEM en AEM Publish para recopilar el contenido. Al recibir una respuesta JSON válida, la respuesta JSON se pasa a cada `ViewBinder` responsable de analizar el JSON y vincularlo a los elementos `View` móviles.

#### Análisis de la respuesta de JSON

A continuación, veremos el `LogoViewBinder`, que es sencillo, pero destaca varias consideraciones importantes.

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

La primera línea de `bind(...)` baja por la respuesta JSON a través de las teclas **:items → raíz → :items** que representa el contenedor de diseño de AEM al que se agregaron los componentes.

Desde aquí se realiza una comprobación para una clave denominada **image**, que representa el componente Imagen (de nuevo, es importante que este nombre de nodo → clave JSON sea estable). Si este objeto existe, lee y asigna al [POJO de imagen personalizado](#image-pojo) a través de la biblioteca Jackson `ObjectMapper`. La imagen POJO es explorada a continuación.

Por último, el `src` del logotipo se carga en el ImageView de Android mediante la biblioteca de ayuda [!DNL Glide].

Tenga en cuenta que debemos proporcionar el esquema de AEM, host y puerto (a través de `aemHost`) a la instancia de publicación de AEM, ya que AEM Content Services solo proporcionará la ruta de JCR (es decir, `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenido al que se hace referencia.

#### La imagen POJO{#image-pojo}

Aunque es opcional, el uso de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) o capacidades similares proporcionadas por otras bibliotecas como Gson, ayuda a asignar estructuras JSON complejas a los POJO de Java sin el tedio de tratar directamente con los propios objetos JSON nativos. En este caso sencillo, asignamos la clave `src` del objeto `image` JSON al atributo `src` del POJO de imagen directamente a través de la anotación `@JSONProperty`.

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

El POJO de eventos, que requiere la selección de muchos más puntos de datos del objeto JSON, se beneficia de esta técnica más que la imagen simple, que todo lo que queremos es el `src`.

## Explorar la experiencia de la aplicación móvil

Ahora que conoce cómo AEM Content Services puede impulsar la experiencia nativa de Mobile, utilice lo que ha aprendido para realizar los siguientes pasos y ver los cambios reflejados en la aplicación móvil.

Después de cada paso, actualice la aplicación móvil y confirme la actualización a la experiencia móvil.

1. Crear y publicar **nuevo [!DNL Event] fragmento de contenido**
1. Cancelar la publicación de un **fragmento de contenido [!DNL Event] existente**
1. Publicar una actualización en la **Línea de etiquetas**

## Felicitaciones

**¡Ha completado el tutorial AEM sin encabezado!**

Para obtener más información sobre AEM Content Services y AEM as a Headless CMS, visite la documentación y otros materiales de habilitación de Adobe:

* [Uso de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guía del usuario de componentes principales de WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [AEM Biblioteca de componentes principales de WCM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM proyecto GitHub de componentes principales de WCM](https://github.com/adobe/aem-core-wcm-components)
* [Componentes principales de WCM AEM: consulte a un experto](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Ejemplo de código del exportador de componentes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
