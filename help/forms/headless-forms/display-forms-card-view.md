---
title: Mostrar los formularios recuperados en la vista de tarjeta
description: Utilice la API listforms para mostrar los formularios
feature: Adaptive Forms
version: 6.5
kt: 13311
topic: Development
role: User
level: Intermediate
exl-id: 7316ca02-be57-4ecf-b162-43a736b992b3
source-git-commit: 3bbf80d5c301953b3a34ef8256702ac7445c40da
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 0%

---

# Buscar y mostrar los formularios en formato de tarjeta

El formato de vista de tarjeta es un patrón de diseño que presenta información o datos en forma de tarjetas. Cada tarjeta representa un fragmento discreto de contenido o entrada de datos y, por lo general, consta de un contenedor visualmente distinto con elementos específicos organizados dentro de él.
Las tarjetas en las que se puede hacer clic en React son componentes interactivos que se asemejan a tarjetas o mosaicos y en los que el usuario puede hacer clic o pulsar. Cuando un usuario pulsa o hace clic en una tarjeta en la que se puede hacer clic, déclencheur una acción o un comportamiento especificados, como navegar a otra página, abrir un modal o actualizar la interfaz de usuario.

En este artículo, utilizaremos el [API de listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms) para recuperar los formularios y mostrarlos en formato de tarjeta y abrir el formulario adaptable en el evento de clic.

![card-view](./assets/card-view-forms.png)

## Plantilla de tarjeta

El siguiente código se utilizó para diseñar la plantilla de tarjeta. La plantilla de tarjeta muestra el título y la descripción del formulario adaptable junto con el logotipo del Adobe. [Componentes de IU de material](https://mui.com/) se han utilizado para crear este diseño.



```javascript
import Container from "@mui/material/Container";
import Form from './Form';
import PlainText from './plainText'
import TextField from './TextField'
import Button from './Button';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { CardActionArea, Typography } from "@mui/material";
import { Box } from "@mui/system";
import { useState,useEffect } from "react";
import DisplayForm from "../DisplayForm";
import { Link } from "react-router-dom";
export default function FormCard({headlessForm}) {
const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    const[formPath, setFormPath] = useState('');
    const [selectedForm, setForm] = useState('');
    return (
        
            <Grid item xs={3}>
                <Paper elevation={3}>
                    <img src="/content/dam/formsanddocuments/registrationform/jcr:content/renditions/cq5dam.thumbnail.48.48.png" className="img"/>
                    <Box padding={3}>
                        <Link style={{ textDecoration: 'none' }} to={`/displayForm${headlessForm.path}`}>
                            <Typography variant="subtititle2" component="h2">
                                {headlessForm.title}
                            </Typography>
                            <Typography variant="subtititle3" component="h4">
                                {headlessForm.description}
                            </Typography>
                        </Link>
                
                    </Box>
                </Paper>
            </Grid>
    );
    

};
```

La siguiente ruta se definió en el archivo Main.js para ir a DisplayForm.js

```javascript
    <Route path="/displayForm/*" element={<DisplayForm/>} exact/>
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

## Pasos siguientes

[Mostrar el formulario adaptable cuando el usuario haga clic en una tarjeta](./open-form-card-view.md)