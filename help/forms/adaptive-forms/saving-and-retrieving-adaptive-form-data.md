---
title: Guardar y recuperar datos de formulario adaptable
description: Guardar y recuperar datos de formulario adaptables de la base de datos. Esta capacidad permite a los rellenadores de formularios guardar el formulario y seguir rellenándolo en un momento posterior.
feature: Adaptive Forms
topic: Development
role: Developer
type: Tutorial
version: 6.4,6.5
last-substantial-update: 2019-06-09T00:00:00Z
duration: 794
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 0%

---


# Guardar y recuperar datos de formulario adaptable

Este artículo le guiará por los pasos necesarios para guardar y recuperar datos de formulario adaptables de la base de datos. La base de datos MySQL se utilizó para almacenar los datos del formulario adaptable. En un nivel superior, los siguientes son los pasos para lograr el caso de uso:

* [Configurar fuente de datos](#Configure-Data-Source)
* [Crear servlet para escribir datos en la base de datos](#create-servlet)
* [Cree el servicio OSGI para recuperar los datos almacenados](#create-osgi-service)
* [Crear biblioteca de cliente](#create-client-library)
* [Crear una plantilla de formulario adaptable y un componente de página](#form-template-and-page-component)
* [Demostración de capacidades](#capability-demo)
* [Implementación en el servidor](#deploy-on-your-server)

## Configurar fuente de datos {#Configure-Data-Source}

La fuente de datos obtenida de una conexión Apache Sling está configurada para que apunte a la base de datos que se utilizará para almacenar los datos del formulario adaptable. La siguiente captura de pantalla muestra la configuración de mi instancia. Las siguientes propiedades se pueden copiar y pegar

* Nombre de origen de datos:aemformstutorial: es el nombre que se usa en mi código.

* Clase de controlador JDBC:com.mysql.jdbc.Driver

* URL de conexión JDBC:jdbc:mysql://localhost:3306/aemformstutorial

![connectionpool](assets/storingdata.PNG)

### Crear servlet {#create-servlet}

El siguiente es el código del servlet que inserta o actualiza los datos del formulario adaptable en la base de datos. AEM La fuente de datos obtenida de una conexión Apache Sling se configura usando el administrador de configuración de la conexión de la red de Apache Sling y se hace referencia a la misma en la línea 26. El resto del código es bastante sencillo. El código inserta una fila nueva en la base de datos o actualiza una fila existente. Los datos del formulario adaptable almacenados están asociados a un GUID. A continuación, se utiliza el mismo GUID para actualizar los datos del formulario.

```java
package com.techmarketing.core.servlets;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;
import javax.servlet.Servlet;
import javax.sql.DataSource;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/storeafdata"})
public class StoreDataInDB extends SlingAllMethodsServlet {
     private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
        private static final long serialVersionUID = 1L;
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
    public String updateData(String afdata,String guid)
    {
         String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
         Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(updateTableSQL);
                pstmt.setString(1,afdata);
                pstmt.setString(2,guid);
                log.debug("Executing the insert statment  " + pstmt.executeUpdate());
                c.commit();
                 
      
            } catch (SQLException e) {
      
                log.error("Getting errors", e);
            } finally {
                if (pstmt != null) {
                    try {
                        pstmt.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
                if (c != null) {
                    try {
                        c.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
            return guid;
     
         
    }
     public String insertData(String afdata) {
            log.debug("### Insert Data #### The json object I got to insert was " + afdata);
            String insertTableSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
            UUID uuid = UUID.randomUUID();
            String randomUUIDString = uuid.toString();
            log.debug("The query is " + insertTableSQL);
            Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(insertTableSQL);
                pstmt.setString(1,randomUUIDString);
                pstmt.setString(2,afdata);
                log.debug("Executing the insert statment  " + pstmt.executeUpdate());
                c.commit();
                 
      
            } catch (SQLException e) {
      
                log.error("Getting errors", e);
            } finally {
                if (pstmt != null) {
                    try {
                        pstmt.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
                if (c != null) {
                    try {
                        c.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
            return randomUUIDString;
        }
      
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.error("not able to get connection ", e);
            }
            return null;
        }
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
    {
        log.debug("Inside my save af data servlet");
        if(request.getParameter("operation").equalsIgnoreCase("update"))
        {
            log.debug("The operation is update");
            log.debug("The data I got was "+request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"),request.getParameter("guid"));
            log.debug("The guid I got was  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(request.getParameter("operation").equalsIgnoreCase("insert"))
        {
            log.debug("The data I got was +request.getParameter("formdata");
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

} catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
 
}
```

## Crear un servicio OSGI para recuperar datos {#create-osgi-service}

El siguiente código se escribió para recuperar los datos del formulario adaptable almacenados. Se utiliza una consulta simple para recuperar los datos del formulario adaptable asociados a un GUID determinado. A continuación, los datos recuperados se devuelven a la aplicación que realiza la llamada. La misma fuente de datos creada en el primer paso a la que se hace referencia en este código.

```java
package com.techmarketing.core.impl;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
import javax.sql.DataSource;
 
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
import com.techmarketing.core.AemFormsAndDB;
 
 
@Component(service=AemFormsAndDB.class,immediate = true)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
 
    @Override
    public String getData(String guid) {
        System.out.println("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '"+guid+"'"+"";
            log.debug(" Got Result Set"+query);
            ResultSet rs = st.executeQuery(query);
            while(rs.next())
            {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
        return null;
    }
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.debug("not able to get connection ");
                e.printStackTrace();
            }
            return null;
        }
 
 
}
```

## Crear biblioteca de cliente {#create-client-library}

AEM La biblioteca de clientes de administra todo el código javascript del lado del cliente. Para este artículo, he creado un javascript simple para recuperar los datos del formulario adaptable mediante la API de Guide Bridge. Una vez recuperados los datos del formulario adaptable, se realiza la llamada al POST al servlet para insertar o actualizar los datos del formulario adaptable en la base de datos. La función getALLUrlParams devuelve los parámetros de la dirección URL. Se utiliza cuando desea actualizar los datos. El resto de la funcionalidad se controla en el código asociado al evento de clic de la clase .savebutton. Si el parámetro guid está presente en la dirección URL, se debe realizar la operación de actualización; en caso contrario, se trata de una operación de inserción.

```javascript
function getAllUrlParams(url) {
 
  // get query string from url (optional) or window
  var queryString = url ? url.split('?')[1] : window.location.search.slice(1);
 
  // we'll store the parameters here
  var obj = {};
 
  // if query string exists
  if (queryString) {
 
    // stuff after # is not part of query string, so get rid of it
    queryString = queryString.split('#')[0];
 
    // split our query string into its component parts
    var arr = queryString.split('&');
 
    for (var i = 0; i < arr.length; i++) {
      // separate the keys and the values
      var a = arr[i].split('=');
 
      // set parameter name and value (use 'true' if empty)
      var paramName = a[0];
      var paramValue = typeof (a[1]) === 'undefined' ? true : a[1];
 
      // (optional) keep case consistent
      paramName = paramName.toLowerCase();
      if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();
 
      // if the paramName ends with square brackets, e.g. colors[] or colors[2]
      if (paramName.match(/\[(\d+)?\]$/)) {
 
        // create key if it doesn't exist
        var key = paramName.replace(/\[(\d+)?\]/, '');
        if (!obj[key]) obj[key] = [];
 
        // if it's an indexed array e.g. colors[2]
        if (paramName.match(/\[\d+\]$/)) {
          // get the index value and add the entry at the appropriate position
          var index = /\[(\d+)\]/.exec(paramName)[1];
          obj[key][index] = paramValue;
        } else {
          // otherwise add the value to the end of the array
          obj[key].push(paramValue);
        }
      } else {
        // we're dealing with a string
        if (!obj[paramName]) {
          // if it doesn't exist, create property
          obj[paramName] = paramValue;
        } else if (obj[paramName] && typeof obj[paramName] === 'string'){
          // if property does exist and it's a string, convert it to an array
          obj[paramName] = [obj[paramName]];
          obj[paramName].push(paramValue);
        } else {
          // otherwise add the property
          obj[paramName].push(paramValue);
        }
      }
    }
  }
 
  return obj;
}
 
$(document).ready(function()
   {
        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
       linktext.visible = false;
       linktext1.visible = false;
        $(".savebutton").click(function(){
           var params = getAllUrlParams(window.location.href);
           console.log(getAllUrlParams(window.location.href));
            window.guideBridge.getDataXML({
                 success:function(guideResultObject) 
                 {
                     console.log("The data is "+guideResultObject.data);
                     let xhr = new XMLHttpRequest();
                      xhr.open('POST','/bin/storeafdata');
                     let formData = new FormData();
                     if(typeof(params.guid)!="undefined")
                     {
                         formData.append("operation","update");
                         formData.append("guid",params.guid);
 
                     }
                     if(typeof(params.guid)=="undefined")
                     {
                         formData.append("operation","insert");
 
 
                     }
 
 
                formData.append("formdata",guideResultObject.data);
                xhr.send(formData);
                     xhr.onload = function(e)
                {
                    console.log("The data is ready");
                    if (this.status == 200)
                        {
                            var jsonResponse = JSON.parse(this.response);
                            console.log(jsonResponse.guid);
                            var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                            var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                            linktext1.visible = true;
                            linktext.value = "http://localhost:4502/content/dam/formsanddocuments/saveformdata/jcr:content?wcmmode=disabled&guid="+jsonResponse.guid;
                            linktext.visible = true;
                            guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        }
 
                }
                  }
             });
 
 
       });
 
 
});
```

## Crear una plantilla de formulario adaptable y un componente de página {#form-template-and-page-component}


>[!VIDEO](https://video.tv.adobe.com/v/27828?quality=12&learn=on)

### Demostración de la capacidad {#capability-demo}

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)

#### Implementación en el servidor {#deploy-on-your-server}

Para probar esta capacidad en su instancia de AEM Forms, siga los siguientes pasos

* [Descargue y descomprima DemoAssets.zip en su sistema local](assets/demoassets.zip)
* Implemente e inicie los paquetes techmarketingdemos.jar y mysqldriver.jar mediante la consola web de Felix.
*** Importe aemformstutorial.sql mediante MYSQL Workbench. Esto creará el esquema y las tablas necesarios en la base de datos
* AEM Importe StoreAndRetrieve.zip mediante el administrador de paquetes de la. Este paquete contiene la plantilla de formulario adaptable, la biblioteca de cliente de componentes de página, el formulario adaptable de ejemplo y la configuración de fuente de datos.
* Inicie sesión en configMgr. Busque &quot;Fuente de datos obtenida de una conexión Apache Sling&quot;. Abra la entrada de fuente de datos asociada a aemformstutorial e introduzca el nombre de usuario y la contraseña específicos de la instancia de base de datos.
* Abra el formulario adaptable
* Complete algunos detalles y haga clic en el botón &quot;Guardar y continuar más tarde&quot;
* Debe recuperar una dirección URL con un GUID en ella.
* Copie la dirección URL y péguela en una nueva pestaña del explorador
* El formulario adaptable debe rellenarse con los datos del paso anterior**
