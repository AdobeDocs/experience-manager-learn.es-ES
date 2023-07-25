---
title: Protección CSRF
description: AEM Obtenga información sobre cómo generar y agregar tokens de CSRF de a las solicitudes de POST, PUT AEM y eliminación permitidas a los usuarios autenticados para que las utilicen de forma.
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---


# Protección CSRF

AEM Obtenga información sobre cómo generar y agregar tokens de CSRF de a las solicitudes de POST, PUT AEM y eliminación permitidas a los usuarios autenticados para que las utilicen de forma.

AEM requiere que se envíe un token CSRF válido para el __autenticado__ __POST__, __PUT o __DELETE__ Solicitudes HTTP a los servicios de AEM Author y Publish.

El token CSRF no es necesario para __GET__ solicitudes, o __anónimo__ solicitudes.

Si no se envía un token CSRF con una solicitud de POST, PUT o DELETE AEM AEM, devuelve un mensaje de respuesta prohibida 403 y, a continuación, registra el error siguiente en el registro de la:

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Consulte la [AEM documentación para obtener más información sobre la protección de CSRF de la](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## Biblioteca de cliente CSRF

AEM proporciona una biblioteca de cliente que se puede utilizar para generar y agregar tokens XHR de CSRF y solicitudes de POST de formularios mediante el parche de funciones de prototipo principales. La funcionalidad la proporciona el `granite.csrf.standalone` categoría de biblioteca de cliente.

Para utilizar este método, agregue `granite.csrf.standalone` como dependencia a la biblioteca de cliente que se carga en la página. Por ejemplo, si está utilizando la variable `wknd.site` categoría de biblioteca de cliente, añadir `granite.csrf.standalone` como dependencia a la biblioteca de cliente que se carga en la página.

## Envío de formulario personalizado con protección CSRF

Si el uso de [`granite.csrf.standalone` biblioteca de cliente](#csrf-client-library) no se puede adaptar a su caso de uso, puede añadir manualmente un token CSRF a un envío de formulario. El siguiente ejemplo muestra cómo agregar un token CSRF a un envío de formulario.

AEM Este fragmento de código muestra cómo, en el envío del formulario, se puede recuperar el token CSRF de la interfaz de usuario de, que se puede agregar a una entrada de formulario denominada `:cq_csrf_token`. Dado que el token CSRF tiene una duración corta, es mejor recuperar y establecer el token CSRF inmediatamente antes de enviar el formulario, lo que garantiza su validez.

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', async (event) => {
    event.preventDefault();

    const form = event.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('afterend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Recuperar con protección CSRF

Si el uso de [`granite.csrf.standalone` biblioteca de cliente](#csrf-client-library) no es compatible con su caso de uso, puede añadir manualmente un token CSRF a un XHR o a las solicitudes de captura. El siguiente ejemplo muestra cómo añadir un token CSRF a un XHR realizado con fetch.

AEM Este fragmento de código muestra cómo recuperar un token CSRF de la base de datos de y cómo agregarlo a la base de datos de una solicitud de recuperación de datos de `CSRF-Token` Encabezado de solicitud HTTP. Dado que el token CSRF tiene una corta duración, es mejor recuperarlo y configurarlo inmediatamente antes de realizar la solicitud de captura, lo que garantiza su validez.

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async function getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}

// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Configuración de Dispatcher

Cuando se utilizan tokens CSRF en el servicio de publicación de AEM, la configuración de Dispatcher debe actualizarse para permitir solicitudes de GET al extremo del token CSRF. La siguiente configuración permite solicitudes de GET al extremo del token CSRF en el servicio de publicación de AEM. Si no se agrega esta configuración, el extremo del token CSRF devuelve una respuesta 404 No encontrado.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
