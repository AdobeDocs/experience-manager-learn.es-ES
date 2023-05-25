---
title: Seleccionar un formulario de una lista de formularios disponibles
description: Utilice la API de listforms para rellenar la lista desplegable
feature: Adaptive Forms
version: 6.5
kt: 13346
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Seleccione un formulario para rellenarlo en una lista desplegable

Las listas desplegables proporcionan una forma compacta y organizada de presentar una lista de opciones a los usuarios. Los elementos de la lista desplegable se rellenarán con los resultados de [API de listforms](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms)

![card-view](./assets/forms-drop-down.png)

## Lista desplegable

El siguiente código se utilizó para rellenar la lista desplegable con los resultados de la llamada a la API de listforms. En función de la selección del usuario, se muestra el formulario adaptable para que el usuario lo rellene y lo envíe. [Componentes de IU de material](https://mui.com/) se han utilizado para crear esta interfaz

```javascript
import * as React from 'react';
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Box from '@mui/material/Box';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import Select, { SelectChangeEvent } from '@mui/material/Select';
import { AdaptiveForm } from "@aemforms/af-react-renderer";

import { useState,useEffect } from "react";
export default function SelectFormFromDropDownList()
 {
    const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };

const[formPath, setFormPath] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The path is "+event.target.value) 
    
        setFormPath(event.target.value)
        console.log("The formPath"+ formPath);
     };
const getForm = async () =>
     {
        const resp = await fetch(`${formPath}/jcr:content/guideContainer.model.json`);
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
     }
const getAFForms =async()=>
     {
        const response = await fetch("/adobe/forms/af/listforms")
        //let myresp = await response.status;
        let myForms = await response.json();
        console.log("Got response"+myForms.items[0].title);
        console.log(myForms.items)
        
        //setFormID('test');
        SetOptions(myForms.items)

        
     }
     useEffect( ()=>{
        getAFForms()
        

    },[]);
    useEffect( ()=>{
        getForm()
        

    },[formPath]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formPath}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.path}>{afForm.title}</MenuItem>
        ))}
        
       
        </Select>
      </FormControl>
      <div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
    </Box>
    

  );
  

}
```

Se utilizaron las dos llamadas de API siguientes al crear esta interfaz de usuario

* [ListForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/List-Forms/operation/listForms). La llamada para recuperar los formularios se realiza una sola vez cuando se procesa el componente. Los resultados de la llamada de API se almacenan en la variable afForms.
En el código anterior, iteramos a través de afForms utilizando la función map y, para cada elemento de la matriz afForms, se crea un componente MenuItem y se agrega al componente Select.

* Buscar formulario: se realiza una llamada de obtención al siguiente extremo, donde formPath es la ruta al formulario adaptable seleccionado por el usuario en la lista desplegable. El resultado de esta llamada de GET se almacena en selectedForm.

```
${formPath}/jcr:content/guideContainer.model.json`
```

* Muestra el formulario seleccionado. El siguiente código se utilizó para mostrar el formulario seleccionado. El elemento AdaptiveForm se proporciona en el paquete npm de aemforms/af-react-renderer y espera las asignaciones y el formJson como sus propiedades

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Pasos siguientes

[Mostrar los formularios en el diseño de tarjeta](./display-forms-card-view.md)



