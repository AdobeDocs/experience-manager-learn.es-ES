---
title: Mostrar los formularios recuperados en la vista de tarjeta
description: Utilice la API listforms para mostrar los formularios
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 0%

---


# Buscar y mostrar los formularios en formato de tarjeta

El formato de vista de tarjeta es un patrón de diseño que presenta información o datos en forma de tarjetas. Cada tarjeta representa un fragmento discreto de contenido o entrada de datos y, por lo general, consta de un contenedor visualmente distinto con elementos específicos organizados dentro de él. En este artículo, utilizaremos el [API de listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) para recuperar los formularios y mostrarlos en formato de tarjeta, como se muestra a continuación

![card-view](./assets/card-view-forms.png)

## Plantilla de tarjeta

El siguiente código se utilizó para diseñar la plantilla de tarjeta. La plantilla de tarjeta muestra el título y la descripción del formulario adaptable junto con el logotipo del Adobe. [Componentes de IU de material](https://mui.com/) se han utilizado para crear este diseño.

```javascript
import Paper from "@mui/material/Paper";
import Grid from "@mui/material/Grid";
import Container from "@mui/material/Container";
import { Typography } from "@mui/material";
import { Box } from "@mui/system";
const FormCard =({headlessForm}) => {
    return (
              <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                    <Typography variant="subtititle2" component="h2">
                        {headlessForm.title}
                    
                    </Typography>
                    <Typography variant="subtititle3" component="h4">
                        {headlessForm.description}
                    
                    </Typography>
                    </Box>
                </Paper>
                </Grid>
          


    );
    

};
export default FormCard;
```

## Buscar los formularios

AEM La API de listforms se utilizaba para recuperar los formularios del servidor de. La API devuelve una matriz de objetos JSON, cada objeto JSON que representa un formulario.

```javascript
import { useState,useEffect } from "react";
import React, { Component } from "react";
import FormCard from "./components/FormCard";
import Grid from "@mui/material/Grid";
import Paper from "@mui/material/Paper";
import Container from "@mui/material/Container";
 
export default function ListForm(){
    const [fetchedForms,SetHeadlessForms] = useState([])
    const getForms=async()=>{
        const response = fetch("/adobe/forms/af/listforms")
        let headlessForms = await (await response).json();
        console.log(headlessForms.items);
        SetHeadlessForms(headlessForms.items);
    }
    useEffect( ()=>{
        getForms()
        

    },[]);
    return(
        <div>
             <div>
                <Container>
                   <Grid container spacing={3}>
                       {
                            fetchedForms.map( (afForm,index) =>
                                <FormCard headlessForm={afForm} key={index}/>
                         
                            )
                        }
                    </Grid>
                </Container>
             </div>

        </div>
    )
}
```

En el código anterior, iteramos a través de fetchedForms utilizando la función map y, para cada elemento de la matriz fetchedForms, se crea un componente FormCard y se agrega al contenedor Grid. Ahora puede utilizar el componente ListForm en la aplicación React según sus necesidades.
