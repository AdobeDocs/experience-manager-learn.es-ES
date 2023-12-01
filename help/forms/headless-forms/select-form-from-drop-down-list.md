---
title: Seleccionar un formulario de una lista de formularios disponibles
description: Utilice la API de listforms para rellenar la lista desplegable
feature: Adaptive Forms
version: 6.5
jira: KT-13346
topic: Development
role: User
level: Intermediate
exl-id: 49b6a172-8c96-4fc6-8d31-c2109f65faac
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 4%

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

const[formID, setFormID] = useState('');
const[afForms,SetOptions] = useState([]);
const [selectedForm, setForm] = useState('');
const HandleChange = (event) =>
     {
        console.log("The form id is "+event.target.value) 
    
        setFormID(event.target.value)
        
     };
const getForm = async () =>
     {
        
        console.log("The formID in getForm"+ formID);
        const resp = await fetch(`/adobe/forms/af/${formID}`);
        let formJSON = await resp.json();
        console.log(formJSON.afModelDefinition);
        setForm(formJSON.afModelDefinition);
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
        

    },[formID]);

  return (
    <Box sx={{ minWidth: 120 }}>
      <FormControl fullWidth>
        <InputLabel id="demo-simple-select-label">Please select the form</InputLabel>
        <Select
          labelId="demo-simple-select-label"
          id="demo-simple-select"
          value={formID}
          label="Please select a form"
          onChange={HandleChange}
          
        >
       {afForms.map((afForm,index) => (
    
        
          <MenuItem  key={index} value={afForm.id}>{afForm.title}</MenuItem>
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

* Formulario de búsqueda: se realiza una llamada de obtención a [getForm](https://opensource.adobe.com/aem-forms-af-runtime/api/#tag/Get-Form-Definition), donde el id es el id del formulario adaptable seleccionado por el usuario en la lista desplegable. El resultado de esta llamada de GET se almacena en selectedForm.

```
const resp = await fetch(`/adobe/forms/af/${formID}`);
let formJSON = await resp.json();
console.log(formJSON.afModelDefinition);
setForm(formJSON.afModelDefinition);
```

* Muestra el formulario seleccionado. El siguiente código se utilizó para mostrar el formulario seleccionado. El elemento AdaptiveForm se proporciona en el paquete npm de aemforms/af-react-renderer y espera las asignaciones y el formJson como sus propiedades

```
<div><AdaptiveForm mappings={extendMappings} formJson={selectedForm}/></div>
```

## Pasos siguientes

[Mostrar los formularios en el diseño de tarjeta](./display-forms-card-view.md)
