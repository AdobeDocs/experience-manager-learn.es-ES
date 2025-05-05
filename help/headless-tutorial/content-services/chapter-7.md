---
title: 'AEM Capítulo 7: Consumo de servicios de contenido desde una aplicación móvil: Servicios de contenido'
description: El capítulo 7 del tutorial ejecuta la aplicación móvil de Android AEM para que consuma contenido creado desde Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 467
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# AEM Capítulo 7: Consumo de servicios de contenido desde una aplicación móvil (en inglés)

El capítulo 7 del tutorial utiliza una aplicación móvil nativa de Android AEM para consumir contenido de los servicios de contenido de la.

## La aplicación móvil de Android

Este tutorial usa una **aplicación nativa simple de Android AEM Mobile** para consumir y mostrar el contenido de eventos expuesto por los servicios de contenido de la aplicación de.

El uso de [Android](https://developer.android.com/) no es muy importante, y la aplicación móvil que la consume se puede escribir en cualquier módulo para cualquier plataforma móvil, por ejemplo, iOS.

Android se utiliza para el tutorial debido a la capacidad de ejecutar un Android AEM Emulator en Windows, macOs y Linux, su popularidad y que puede escribirse como Java, un lenguaje bien entendido por los desarrolladores de.

*La aplicación móvil de Android del tutorial **no está**&#x200B;pensada para indicar cómo generar aplicaciones móviles de Android o transmitir las prácticas recomendadas de desarrollo de Android AEM, sino para ilustrar cómo se pueden consumir los servicios de contenido de la aplicación móvil.*

### AEM Cómo impulsan los servicios de contenido la experiencia de la aplicación móvil

![Asignación de aplicación móvil a servicios de contenido](assets/chapter-7/content-services-mapping.png)

1. El **logotipo** definido por el **componente de imagen** de la página [!DNL Events API].
1. La **línea de etiqueta** tal como se define en el **componente de texto** de la página [!DNL Events API].
1. Esta **lista de eventos** se deriva de la serialización de los fragmentos de contenido de eventos, expuestos a través del **componente de lista de fragmentos de contenido** configurado.

## Demostración de aplicaciones móviles

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Configuración de la aplicación móvil para un uso de host no local

AEM Si Publish AEM no se está ejecutando en **http://localhost:4503**, el host y el puerto se pueden actualizar en [!DNL Settings] de la aplicación móvil para que apunten a la propiedad host/puerto de Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Ejecución local de la aplicación móvil

1. Descargue e instale [Android Studio](https://developer.android.com/studio/install) para instalar el emulador de Android.
1. **Descargar** el archivo de Android [!DNL APK] [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abrir **Android Studio**
   * Al iniciar Android Studio por primera vez, aparecerá un mensaje para instalar [!DNL Android SDK]. Acepte los valores predeterminados y finalice la instalación.
1. Abra Android Studio y seleccione **Perfil o Depurar APK**
1. Seleccione el archivo APK (**wknd-mobile.x.x.x.apk**) descargado en el paso 2 y haga clic en **Aceptar**
   * Si se le pide que **cree una nueva carpeta** o que **use la existente**, seleccione **usar existente**.
1. En el primer inicio de Android Studio, haga clic con el botón derecho en **wknd-mobile.x.x.x** de la lista Proyectos y seleccione **Abrir configuración del módulo**.
   * En **Módulos > wknd-mobile.x.x.x > pestaña Dependencias**, seleccione **Plataforma de API 29 de Android**. Pulse Aceptar para cerrar y guardar los cambios.
   * Si no lo hace, verá el error &quot;Seleccione el SDK de Android&quot; al intentar iniciar el emulador.
1. Abra **Administrador AVD** seleccionando **Herramientas > Administrador AVD** o pulsando el icono **Administrador AVD** en la barra superior.
1. En la ventana **Administrador AVD**, haga clic en **+ Crear dispositivo virtual...** si aún no tiene el dispositivo registrado.
   1. En el lado izquierdo, seleccione la categoría **Teléfono**.
   1. Seleccione un **píxel 2**.
   1. Haga clic en el botón **Siguiente**.
   1. Seleccione **Q** con **nivel de API 29**.
      * Tras el primer inicio de AVD Manager, se le pedirá que Descargue la API con versión. Haga clic en el vínculo Descargar junto a la versión &quot;Q&quot; y complete la descarga y la instalación.
   1. Haga clic en el botón **Siguiente**.
   1. Haga clic en el botón **Finalizar**.
1. Cierre la ventana **Administrador AVD**.
1. En la barra de menú superior, seleccione **wknd-mobile.x.x.x** de la lista desplegable **Ejecutar/Editar configuraciones**.
1. Pulse el botón **Ejecutar** junto a la **Configuración de ejecución/edición** seleccionada
1. En la ventana emergente, seleccione el dispositivo virtual **[!DNL Pixel 2 API 29]** recién creado y pulse **Aceptar**
1. Si la aplicación [!DNL WKND Mobile] no se carga inmediatamente, busque y pulse el icono **[!DNL WKND]** en la pantalla de inicio de Android en el emulador.
   * Si el emulador se inicia pero su pantalla permanece en negro, pulse el botón **power** de la ventana de herramientas del emulador situada junto a la ventana del emulador.
   * Para desplazarse dentro del dispositivo virtual, haga clic y mantenga presionado el ratón y arrastre.
   * AEM Para actualizar el contenido desde el punto de vista de la actualización, tire hacia abajo desde la parte superior hasta el icono Actualizar.
muestra, y la versión.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## El código de la aplicación móvil

En esta sección se destaca el código de la aplicación móvil de Android AEM que más interactúa y depende de los servicios de contenido de la aplicación y de su salida JSON.

AEM Tras la carga, la aplicación móvil realiza `HTTP GET` en `/content/wknd-mobile/en/api/events.model.json`, que es el extremo de Content Services configurado para proporcionar contenido que dirija la aplicación móvil.

Debido a que la plantilla editable de la API de eventos (`/content/wknd-mobile/en/api/events.model.json`) está bloqueada, la aplicación móvil se puede codificar para buscar información específica en ubicaciones específicas en la respuesta JSON.

### Flujo de código de alto nivel

1. AEM Al abrir la aplicación [!DNL WKND Mobile], se invoca una solicitud de `HTTP GET` a la Publish de la aplicación móvil en `/content/wknd-mobile/en/api/events.model.json` para recopilar el contenido y rellenar la interfaz de usuario de la aplicación móvil.
2. AEM AEM Una vez recibido el contenido de la aplicación móvil, cada uno de los tres elementos de vista de la aplicación móvil, el logotipo **y la línea de etiquetas y la lista de eventos**, se inicializan con el contenido de la aplicación de la aplicación móvil, que se ha iniciado con el contenido de la lista de eventos de la aplicación de la aplicación móvil de la aplicación de la aplicación de la aplicación de la aplicación móvil, la etiqueta y la lista de eventos de la aplicación de la.
   * AEM AEM Para enlazar al contenido de la aplicación móvil al elemento de vista, el JSON que representa cada componente de la aplicación móvil, es un objeto asignado a un POJO de Java, que a su vez está enlazado al elemento de vista de Android.
      * Componente de imagen JSON → Logo POJO → Logo ImageView
      * Componente de texto JSON → TagLine POJO → Texto ImageView
      * Lista de fragmentos de contenido JSON → eventos POJO →Events RecyclerView
   * *El código de la aplicación móvil puede asignar el JSON a los POJO debido a las ubicaciones bien conocidas dentro de la respuesta JSON mayor. AEM Recuerde, las claves JSON de &quot;image&quot;, &quot;text&quot; y &quot;contentfragmentlist&quot; están dictadas por los nombres de nodo de los componentes de la de respaldo. Si estos nombres de nodo cambian, la aplicación móvil se romperá, ya que no sabrá cómo obtener el contenido requerido de los datos JSON.*

#### AEM Invocación del punto final de servicios de contenido de

AEM A continuación se muestra una destilación del código de `MainActivity` de la aplicación móvil responsable de invocar los servicios de contenido de la aplicación para recopilar el contenido que impulsa la experiencia de la aplicación móvil.

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

`onCreate(..)` es el vínculo de inicialización para la aplicación móvil y registra a los 3 `ViewBinders` personalizados responsables de analizar el JSON y enlazar los valores a los elementos `View`.

A continuación, se llama a `initApp(...)`, que realiza la solicitud de GET AEM AEM HTTP al punto final de los servicios de contenido de la en la interfaz de usuario de Publish para recopilar el contenido de la aplicación. Al recibir una respuesta JSON válida, la respuesta JSON se pasa a cada `ViewBinder` responsable de analizar el JSON y enlazarlo a los elementos móviles `View`.

#### Análisis de la respuesta JSON

A continuación veremos `LogoViewBinder`, que es simple, pero resalta varias consideraciones importantes.

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

AEM La primera línea de `bind(...)` navega hacia abajo en la respuesta JSON a través de las claves **:items → root → :items**, que representa el contenedor de diseño de la al que se agregaron los componentes.

Desde aquí se realiza una comprobación de una clave denominada **image**, que representa el componente de imagen (de nuevo, es importante que este nombre de nodo → clave JSON sea estable). Si este objeto existe, se leerá y se asignará al [POJO de imagen personalizado](#image-pojo) a través de la biblioteca Jackson `ObjectMapper`. El POJO de imagen se explora a continuación.

Por último, `src` del logotipo se carga en Android ImageView mediante la biblioteca de ayuda [!DNL Glide].

AEM AEM Tenga en cuenta que debemos proporcionar el esquema, el host y el puerto de la (a través de `aemHost`) a la instancia de Publish AEM, ya que los servicios de contenido solo proporcionarán la ruta JCR (es decir, los servicios de contenido). `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenido referenciado.

#### El POJO de imagen{#image-pojo}

Aunque es opcional, el uso de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) o capacidades similares proporcionadas por otras bibliotecas, como Gson, ayuda a asignar estructuras JSON complejas a POJO de Java sin el tedio de tratar directamente con los propios objetos JSON nativos. En este caso simple, asignamos la clave `src` desde el objeto JSON `image` al atributo `src` en el POJO de imagen directamente a través de la anotación `@JSONProperty`.

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

El POJO de evento, que requiere la selección de muchos más puntos de datos del objeto JSON, se beneficia de esta técnica más que de la imagen simple, que solo queremos que sea `src`.

## Explorar la experiencia de la aplicación móvil

AEM Ahora que comprende cómo los servicios de contenido de pueden impulsar la experiencia nativa en dispositivos móviles, utilice lo que ha aprendido para realizar los siguientes pasos y ver los cambios reflejados en la aplicación móvil.

Después de cada paso, tire de la aplicación móvil para actualizarla y verifique la actualización de la experiencia móvil.

1. Crear y publicar **nuevo [!DNL Event] fragmento de contenido**
1. Cancelar la publicación de un **fragmento de contenido [!DNL Event] existente**
1. Publish puede actualizar la **línea de etiquetas**

## Felicitaciones

AEM **Ha completado el tutorial sin encabezado de la aplicación de la aplicación de la aplicación de la aplicación de seguridad de!**

AEM AEM Para obtener más información acerca de los servicios de contenido de la y la configuración de un CMS sin encabezado, visite otra documentación y materiales de habilitación de Adobe:

* [Uso de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html?lang=es)
* AEM [Guía del usuario de componentes principales de WCM de](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* AEM [Biblioteca de componentes principales de WCM de](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* AEM [Proyecto de GitHub de componentes principales de WCM de](https://github.com/adobe/aem-core-wcm-components)
* [Muestra de código del exportador de componentes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
