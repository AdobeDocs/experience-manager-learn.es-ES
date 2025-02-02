---
title: Creación de componentes de imagen en los que se puede hacer clic
description: Creación de un componente de imagen en el que se puede hacer clic en AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 54344a6d-51d3-4a63-b1f1-283bddbc0f8f
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 0%

---

# Controlar evento de clic

Cree una biblioteca de cliente y asóciela con el componente.

Agregue el siguiente código en su archivo javascript de la biblioteca de cliente para controlar el evento click.
En función del estado seleccionado, se pueden mostrar los datos adecuados devueltos por el punto final. Los detalles del punto de conexión y los datos que se van a mostrar dependerán de su caso de uso.



```javascript
document.addEventListener("DOMContentLoaded", function(event)
  {
    const apiUrl = 'API end point to return data based on the state selected';
       // Select all <path> elements
    const paths = document.querySelectorAll('path');
    const tooltip = document.getElementById('tooltip');
    const svg = document.getElementById('svg');
    const states = document.querySelectorAll('path');
    // Loop through each <path> element and add an event listener
    states.forEach(state =>
     {
                     state.addEventListener('click', function(event)
                  {
                     alert('path clicked:'+ this.id);
                    fetch(apiUrl+this.id)
                    .then(response =>
                        {
                            // Check if the response is ok (status code in the range 200-299)
                            if (!response.ok)
                            {
                                  throw new Error('Network response was not ok ' + response.statusText);
                            }
                            // Parse the JSON from the response
                            console.log(response);
                            return response.text();
                          })
                        .then(data => {
                                // Handle the data
                              console.log(data);
                            })
                        .catch(error => {
                            // Handle any errors
                            console.error('There was a problem with the fetch operation:', error);
                            });
                     });
        });

});
```
