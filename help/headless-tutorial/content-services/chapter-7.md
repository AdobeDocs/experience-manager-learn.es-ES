---
title: 'AEM Capítulo 7: Consumo de servicios de contenido desde una aplicación móvil: Servicios de contenido'
description: AEM El capítulo 7 del tutorial ejecuta la aplicación móvil de Android para que consuma contenido creado desde los servicios de contenido de la aplicación de.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 512
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# AEM Capítulo 7: Consumo de servicios de contenido desde una aplicación móvil (en inglés)

AEM El capítulo 7 del tutorial utiliza una aplicación móvil nativa de Android para consumir contenido de los servicios de contenido de los servicios de contenido de la aplicación de la aplicación de contenido de.

## La aplicación móvil de Android

Este tutorial utiliza un **aplicación móvil nativa simple de Android** AEM para consumir y mostrar el contenido de evento expuesto por los servicios de contenido de.

El uso de [Android](https://developer.android.com/) no es importante en gran medida, y la aplicación móvil consumidora podría escribirse en cualquier marco de trabajo para cualquier plataforma móvil, por ejemplo iOS.

AEM Android se utiliza para el tutorial debido a la capacidad de ejecutar un emulador de Android en Windows, macOs y Linux, su popularidad, y que puede escribirse como Java, un lenguaje bien entendido por los desarrolladores de.

*La aplicación móvil de Android del tutorial es **no**AEM tiene la intención de instruir cómo crear aplicaciones móviles de Android o transmitir las prácticas recomendadas de desarrollo de Android, sino más bien ilustrar cómo se pueden consumir los servicios de contenido de desde una aplicación móvil.*

### AEM Cómo impulsan los servicios de contenido la experiencia de la aplicación móvil

![Asignación de aplicación móvil a servicios de contenido](assets/chapter-7/content-services-mapping.png)

1. El **logo** tal como se define en [!DNL Events API] de la página **Componente de imagen**.
1. El **línea de etiquetas** tal como se define en la [!DNL Events API] de la página **Componente Texto**.
1. Esta **Lista de eventos** se deriva de la serialización de los fragmentos de contenido de evento, expuestos a través del configurado **Componente Lista de fragmentos de contenido**.

## Demostración de aplicaciones móviles

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Configuración de la aplicación móvil para un uso de host no local

AEM Si no se ejecuta la publicación de la en **http://localhost:4503** el host y el puerto se pueden actualizar en el [!DNL Settings] AEM para que apunte a la propiedad Publish host/port.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Ejecución local de la aplicación móvil

1. Descargue e instale [Android Studio](https://developer.android.com/studio/install) para instalar el emulador de Android.
1. **Descargar** el Android [!DNL APK] archivo [GitHub > Recursos > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abrir **Android Studio**
   * En el primer inicio de Android Studio, se mostrará un mensaje para instalar el [!DNL Android SDK] se presentará. Acepte los valores predeterminados y finalice la instalación.
1. Abra Android Studio y seleccione **APK de perfil o depuración**
1. Seleccione el archivo APK (**wknd-mobile.x.x.x.apk**) en el paso 2 y haga clic en **OK**
   * Si se le solicita **Crear una nueva carpeta**, o **Usar existente**, seleccione **Usar existente**.
1. En el primer inicio de Android Studio, haga clic con el botón derecho en el **wknd-mobile.x.x.x** en la lista Proyectos y seleccione **Abrir configuración del módulo**.
   * En **Módulos > wknd-mobile.x.x.x > pestaña Dependencias**, seleccione **Plataforma de API 29 de Android**. Pulse Aceptar para cerrar y guardar los cambios.
   * Si no lo hace, verá el error &quot;Seleccione el SDK de Android&quot; al intentar iniciar el emulador.
1. Abra el **Administrador AVD** seleccionando **Herramientas > Administrador AVD** o pulsando el botón **Administrador AVD** en la barra superior.
1. En el **Administrador AVD** , haga clic en **+ Crear dispositivo virtual...** si aún no tiene el dispositivo registrado.
   1. En la parte izquierda, seleccione **Teléfono** categoría.
   1. Seleccione una **Píxel 2**.
   1. Haga clic en **Siguiente** botón.
   1. Seleccionar **Q** con **Nivel de API 29**.
      * Tras el primer inicio de AVD Manager, se le pedirá que Descargue la API con versión. Haga clic en el vínculo Descargar junto a la versión &quot;Q&quot; y complete la descarga y la instalación.
   1. Haga clic en **Siguiente** botón.
   1. Haga clic en **Finalizar** botón.
1. Cierre el **Administrador AVD** ventana.
1. En la barra de menús superior, seleccione **wknd-mobile.x.x.x** desde el **Ejecutar/Editar configuraciones** desplegable.
1. Pulse el botón **Ejecutar** junto al botón seleccionado **Ejecutar/Editar configuración**
1. En la ventana emergente, seleccione el elemento recién creado **[!DNL Pixel 2 API 29]** dispositivo virtual y toque **OK**
1. Si la variable [!DNL WKND Mobile] La aplicación no se carga, busca y toca inmediatamente en **[!DNL WKND]** de la pantalla de inicio de Android en el emulador.
   * Si el emulador se inicia pero la pantalla del emulador permanece negra, pulse el botón **poder** en la ventana de herramientas del emulador, junto a la ventana del emulador.
   * Para desplazarse dentro del dispositivo virtual, haga clic y mantenga presionado el ratón y arrastre.
   * AEM Para actualizar el contenido desde la ubicación de inicio, tire hacia abajo desde la parte superior hasta que aparezca el icono Actualizar y suelte.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## El código de la aplicación móvil

AEM Esta sección resalta el código de la aplicación móvil de Android que más interactúa y depende de los servicios de contenido y de su salida JSON.

Al cargar, la aplicación móvil realiza lo siguiente `HTTP GET` hasta `/content/wknd-mobile/en/api/events.model.json` AEM que es el punto final de Content Services de configurado para proporcionar el contenido que dirigirá la aplicación móvil.

Debido a que la plantilla editable de la API de eventos (`/content/wknd-mobile/en/api/events.model.json`) está bloqueado, la aplicación móvil se puede codificar para buscar información específica en ubicaciones específicas en la respuesta JSON.

### Flujo de código de alto nivel

1. Abriendo el [!DNL WKND Mobile] La aplicación invoca un `HTTP GET` AEM solicitud a la publicación de la en `/content/wknd-mobile/en/api/events.model.json` para recopilar el contenido y rellenar la interfaz de usuario de la aplicación móvil.
2. AEM Una vez recibido el contenido de la aplicación móvil, cada uno de los tres elementos de vista de la aplicación móvil, el elemento de visualización de la aplicación móvil se muestra como: **logotipo, línea de etiquetas y lista de eventos** AEM , se inicializan con el contenido de la lista de distribución de.
   * AEM AEM Para enlazar al contenido de la aplicación móvil al elemento de vista, el JSON que representa cada componente de la aplicación móvil, es un objeto asignado a un POJO de Java, que a su vez está enlazado al elemento de vista de Android.
      * Componente de imagen JSON → Logo POJO → Logo ImageView
      * Componente de texto JSON → TagLine POJO → Texto ImageView
      * Lista de fragmentos de contenido JSON → eventos POJO →Events RecyclerView
   * *El código de la aplicación móvil puede asignar el JSON a los POJO debido a las ubicaciones bien conocidas dentro de la respuesta JSON mayor. AEM Recuerde, las claves JSON de &quot;image&quot;, &quot;text&quot; y &quot;contentfragmentlist&quot; están dictadas por los nombres de nodo de los componentes de la de respaldo. Si estos nombres de nodo cambian, la aplicación móvil se interrumpirá, ya que no sabrá cómo obtener el contenido requerido de los datos JSON.*

#### AEM Invocación del punto final de servicios de contenido de

A continuación se muestra una destilación del código en el `MainActivity` AEM es responsable de invocar los servicios de contenido de para recopilar el contenido que impulsa la experiencia de la aplicación móvil.

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

`onCreate(..)` es el vínculo de inicialización para la aplicación móvil y registra los 3 `ViewBinders` responsable de analizar el JSON y enlazar los valores al `View` elementos.

`initApp(...)` A continuación, se llama a, que realiza la solicitud de GET AEM AEM HTTP al punto final de los servicios de contenido de la en la publicación de la aplicación para recopilar el contenido. Al recibir una respuesta JSON válida, la respuesta JSON se pasa a cada `ViewBinder` que es responsable de analizar el JSON y enlazarlo al dispositivo móvil `View` elementos.

#### Análisis de la respuesta JSON

A continuación, veremos el `LogoViewBinder`, que es simple, pero resalta varias consideraciones importantes.

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

La primera línea de `bind(...)` navega hacia abajo en la respuesta JSON a través de las teclas **:items → root → :items** AEM que representa el contenedor de diseño de la al que se agregaron los componentes.

Desde aquí se realiza una comprobación de una clave denominada **imagen**, que representa el componente Imagen (de nuevo, es importante que este nombre de nodo → clave JSON sea estable). Si este objeto existe, se lee y se asigna al [POJO de imagen personalizado](#image-pojo) a través de Jackson `ObjectMapper` biblioteca. El POJO de imagen se explora a continuación.

Por último, el logotipo de `src` se carga en la ImageView de Android mediante la variable [!DNL Glide] biblioteca de ayuda.

AEM Tenga en cuenta que debemos proporcionar el esquema, el host y el puerto de la (a través de ). `aemHost`AEM AEM ) a la instancia de publicación de la, ya que los servicios de contenido solo proporcionarán la ruta JCR (es decir, `/content/dam/wknd-mobile/images/wknd-logo.png`) al contenido referenciado.

#### El POJO de imagen{#image-pojo}

Si bien es opcional, el uso de la variable [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) o funciones similares proporcionadas por otras bibliotecas, como Gson, ayuda a asignar estructuras JSON complejas a POJO de Java sin el tedio de tratar directamente con los propios objetos JSON nativos. En este caso sencillo asignamos el `src` clave de la `image` Objeto JSON, al `src` en el POJO de imagen directamente a través de `@JSONProperty` anotación.

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

El POJO de evento, que requiere la selección de muchos más puntos de datos del objeto JSON, se beneficia de esta técnica más que de la simple imagen, que todo lo que queremos es la `src`.

## Explorar la experiencia de la aplicación móvil

AEM Ahora que comprende cómo los servicios de contenido de pueden impulsar la experiencia nativa en dispositivos móviles, utilice lo que ha aprendido para realizar los siguientes pasos y ver los cambios reflejados en la aplicación móvil.

Después de cada paso, tire de la aplicación móvil para actualizarla y verifique la actualización de la experiencia móvil.

1. Crear y publicar **nuevo [!DNL Event] Fragmento de contenido**
1. Cancelar la publicación de un **existente [!DNL Event] Fragmento de contenido**
1. Publicación de una actualización en **Línea de etiqueta**

## Felicitaciones

**AEM ¡Ha completado el tutorial sin encabezado de la!**

AEM AEM Para obtener más información acerca de los servicios de contenido de la y la configuración de un CMS sin encabezado, visite otra documentación y materiales de habilitación de Adobe:

* [Uso de fragmentos de contenido](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM Guía del usuario de componentes principales de WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=es)
* [AEM Biblioteca de componentes de componentes principales de WCM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM Proyecto de GitHub de componentes principales de WCM](https://github.com/adobe/aem-core-wcm-components)
* [Muestra de código del exportador de componentes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
