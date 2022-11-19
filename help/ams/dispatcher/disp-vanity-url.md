---
title: AEM función de URL mnemónicas de Dispatcher
description: Comprender cómo AEM trata las URL mnemónicas y las técnicas adicionales usando reglas de reescritura para asignar contenido más cerca del límite del envío.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---


# URL mnemónicas de Dispatcher

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Vaciado de Dispatcher](./disp-flushing.md)

## Información general

Este documento le ayudará a comprender cómo AEM trata las direcciones URL mnemónicas y algunas técnicas adicionales mediante reglas de reescritura para asignar contenido más cerca del límite del envío

## ¿Qué son las URL mnemónicas?

Cuando tiene contenido que reside en una estructura de carpetas que tiene sentido, no siempre vive en una URL que sea fácil de consultar.  Las URL mnemónicas son como accesos directos.  Direcciones URL únicas o más cortas que hacen referencia a dónde se encuentra el contenido real.

Un ejemplo: `/aboutus` apuntado `/content/we-retail/us/en/about-us.html`

Los autores de AEM tienen la opción de establecer propiedades de URL de vanidad en un fragmento de contenido en AEM y publicarlo.

Para que esta función funcione, tendrá que ajustar los filtros de Dispatcher para permitir que la vanidad pase.  Esto no es razonable con el ajuste de los archivos de configuración de Dispatcher a la velocidad que los autores necesitarían para configurar estas entradas de página mnemónicas.

Por este motivo, el módulo de Dispatcher tiene una función para permitir automáticamente cualquier elemento enumerado como elemento mnemónico en el árbol de contenido.


## Funcionamiento

### Creación de direcciones URL mnemónicas

El autor visita una página en AEM, visita las propiedades de la página y añade las entradas en la sección de la URL mnemónica.

Una vez que guardan los cambios y activan la página, la vanidad se asigna ahora a esta página.

#### IU táctil:

![Menú desplegable del cuadro de diálogo para AEM IU de creación en la pantalla del editor del sitio](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![página de diálogo de propiedades de página de aem](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### Buscador de contenido clásico:

![AEM propiedades de la página de la barra de tareas clásica de siteadmin](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Cuadro de diálogo de propiedades de página de la IU clásica](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Por favor, entiendan que esto es muy propenso a nombrar problemas de espacio.

Las entradas mnemónicas son globales para todas las páginas, esta es solo una de las deficiencias que tiene que planificar para encontrar soluciones que explicaremos más adelante.
</div>

## Asignación/resolución de recursos

Cada entrada mnemónica es una entrada de mapa sling para una redirección interna.

Estos mapas son visibles visitando la consola Felix de instancias de AEM ( `/system/console/jcrresolver` )

A continuación se muestra una captura de pantalla de una entrada de mapa creada por una entrada mnemónica:
![captura de pantalla de la consola de una entrada mnemónica en las reglas de resolución de recursos](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

En el ejemplo anterior, cuando pedimos a la instancia de AEM que visite `/aboutus` se resolverá `/content/we-retail/us/en/about-us.html`

## Filtros de autoautorización de Dispatcher

Dispatcher en un estado seguro filtra las solicitudes en la ruta `/` a través de Dispatcher porque esa es la raíz del árbol JCR.

Es importante asegurarse de que los editores solo permitan contenido de la variable `/content` y otros caminos seguros, etc.  y no rutas como `/system` etc.

Aquí están las URL mnemónicas activas en la carpeta base de `/` así que, ¿cómo les permitimos llegar a los editores mientras se mantienen seguros?

Simple Dispatcher tiene un mecanismo de autorización de filtro automático y debe instalar un paquete AEM y luego configurar Dispatcher para que apunte a esa página de paquetes.

[https://experience.adobe.com/#/downloads/content/software-distribution/es/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/es/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher tiene una sección de configuración en su archivo de granja:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

Esta configuración le indica a Dispatcher que recupere esta dirección URL de su instancia AEM que se abre cada 300 segundos para recuperar la lista de elementos que queremos permitir.

Almacena su caché de la respuesta en la variable `/file` argumento so en este ejemplo `/tmp/vanity_urls`

Por lo tanto, si visita la instancia de AEM en la URI, verá lo que obtiene:
![captura de pantalla del contenido representado en /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

Es literalmente una lista, súper simple

## Reescribir reglas como reglas mnemónicas

¿Por qué mencionamos el uso de reglas de reescritura en lugar del mecanismo predeterminado integrado en AEM como se describe más arriba?

Se explican simplemente los problemas de espacio de nombres, el rendimiento y la lógica de nivel superior que se pueden administrar mejor.

Veamos un ejemplo de la entrada mnemónica `/aboutus` a su contenido `/content/we-retail/us/en/about-us.html` uso de Apache `mod_rewrite` para lograrlo.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Esta regla buscará la vanidad `/aboutus` y obtenga la ruta completa del procesador con el indicador PT (Pasar).

También dejará de procesar todas las demás reglas del indicador L (Última), lo que significa que no tendrá que atravesar una enorme lista de reglas como la resolución JCR tiene que hacer.

Además de no tener que representar la solicitud y esperar a que el editor de AEM responda a estos dos elementos de este método, se hace mucho más eficaz.

Entonces la guinda del pastel aquí es la bandera NC (sin distinción de mayúsculas y minúsculas), que significa si un cliente cambia la URI por `/AboutUs` en lugar de `/aboutus` seguirá funcionando y permitirá que se recupere la página correcta.

Para crear una regla de reescritura para ello, debe crear un archivo de configuración en Dispatcher (ejemplo: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) e inclúyalo en el `.vhost` que gestiona el dominio que necesita aplicar estas URL mnemónicas.

Este es un ejemplo de fragmento de código de la inclusión dentro de `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## Qué método y dónde

El uso de AEM para controlar las entradas mnemónicas tiene las siguientes ventajas
- Los autores pueden crearlos sobre la marcha
- Viven con el contenido y se pueden empaquetar con él

Uso `mod_rewrite` para controlar las entradas mnemónicas tiene las siguientes ventajas
- Resolución de contenido más rápida
- Más cerca del límite de las solicitudes de contenido del usuario final
- Más extensibilidad y opciones para controlar cómo se asigna el contenido a otras condiciones
- Puede distinguir entre mayúsculas y minúsculas

Utilice ambos métodos, pero aquí están los consejos y criterios que debe utilizar cuando:
- Si el elemento mnemónico es temporal y tiene un tráfico planificado bajo, utilice la función integrada AEM
- Si el elemento mnemónico es un punto final básico que no cambia con frecuencia y tiene uso frecuente, utilice un `mod_rewrite` regla.
- Si el área de nombres de vanidad (por ejemplo: `/aboutus`) debe reutilizarse para muchas marcas en la misma instancia de AEM y, a continuación, utilizar reglas de reescritura.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Si desea utilizar la función mnemónica AEM y evitar el área de nombres, puede crear una convención de nombres.  Uso de URL mnemónicas anidadas como `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[Siguiente -> Registro común](./common-logs.md)