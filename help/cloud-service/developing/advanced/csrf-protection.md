---
title: Protección CSRF
description: Obtenga información sobre cómo generar y agregar tokens de CSRF de AEM a las solicitudes de POST, PUT y Delete permitidas a AEM para usuarios autenticados.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
duration: 125
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# Protección CSRF

Obtenga información sobre cómo generar y agregar tokens de CSRF de AEM a las solicitudes de POST, PUT y Delete permitidas a AEM para usuarios autenticados.

AEM requiere que se envíe un token CSRF válido para las solicitudes HTTP __autenticadas__ __POST__, __PUT o __DELETE__ a los servicios AEM Author y Publish.

El token CSRF no es necesario para __solicitudes GET__ o __solicitudes anónimas__.

Si no se envía un token CSRF con una petición POST, PUT o DELETE, AEM devuelve una respuesta prohibida 403 y AEM registra el siguiente error:

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Consulte la [documentación de para obtener más información sobre la protección CSRF de AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=es).


## Biblioteca de cliente CSRF

AEM proporciona una biblioteca de cliente que se puede utilizar para generar y agregar tokens XHR de CSRF y formar solicitudes POST mediante el parche de funciones de prototipo principales. La funcionalidad la proporciona la categoría de biblioteca de cliente `granite.csrf.standalone`.

Para utilizar este enfoque, agregue `granite.csrf.standalone` como dependencia a la biblioteca de cliente que se carga en su página. Por ejemplo, si está usando la categoría de biblioteca de cliente `wknd.site`, agregue `granite.csrf.standalone` como dependencia a la biblioteca de cliente que se carga en su página.

## Envío de formulario personalizado con protección CSRF

Si el uso de la biblioteca de cliente [`granite.csrf.standalone` &#x200B;](#csrf-client-library) no se ajusta a su caso de uso, puede agregar manualmente un token CSRF a un envío de formulario. El siguiente ejemplo muestra cómo agregar un token CSRF a un envío de formulario.

Este fragmento de código muestra cómo, en el envío del formulario, se puede recuperar el token CSRF desde AEM y agregarlo a una entrada de formulario denominada `:cq_csrf_token`. Dado que el token CSRF tiene una duración corta, es mejor recuperar y establecer el token CSRF inmediatamente antes de enviar el formulario, lo que garantiza su validez.

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
        form.insertAdjacentHTML('beforeend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Recuperar con protección CSRF

Si el uso de la biblioteca de cliente [`granite.csrf.standalone` &#x200B;](#csrf-client-library) no se puede permitir para su caso de uso, puede agregar manualmente un token CSRF a un XHR o a las solicitudes de recuperación. El siguiente ejemplo muestra cómo añadir un token CSRF a un XHR realizado con fetch.

Este fragmento de código muestra cómo recuperar un token CSRF de AEM y añadirlo al encabezado de solicitud HTTP `CSRF-Token` de una solicitud de captura. Dado que el token CSRF tiene una corta duración, es mejor recuperarlo y configurarlo inmediatamente antes de realizar la solicitud de captura, lo que garantiza su validez.

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

Al utilizar tokens CSRF en el servicio de publicación de AEM, la configuración de Dispatcher debe actualizarse para permitir solicitudes de GET al extremo del token CSRF. La siguiente configuración permite solicitudes de GET al extremo del token CSRF en el servicio de publicación de AEM. Si no se agrega esta configuración, el extremo del token CSRF devuelve una respuesta 404 No encontrado.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
