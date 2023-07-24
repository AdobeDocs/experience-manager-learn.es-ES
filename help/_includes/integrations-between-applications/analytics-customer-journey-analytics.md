---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Integración de Adobe Analytics con Customer Journey Analytics

{{analytics-description}}

{{customer-journey-analytics-description}}

La integración de Adobe Analytics con Customer Journey Analytics ofrece ventajas clave:

+ **Perspectivas completas** en los comportamientos y preferencias del cliente.
+ **Seguimiento en canales múltiples sin problemas** para obtener una vista integral.
+ **Datos unificados e informes** para un análisis preciso.
+ **Personalización mejorada** y una participación del cliente mejorada.
+ **Perspectivas de datos en tiempo real** para una toma de decisiones ágil.

## Integraciones comunes

<table>
    <thead>
        <tr>
            <td>aplicaciones de Experience Cloud</td>
            <td>Se integra mediante</td>
            <td>Cuándo se usa</td>
            <td>Casos de uso comunes</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics con Customer Journey Analytics</a></td>
            <td>Conector de origen del Experience Platform</td>
            <td>
                <ul>
                    <li>TODO: Utilice esta integración para introducir datos de Analytics en Experience Platform desde sus grupos de informes.</li>
                    <li>Cuando la disponibilidad de los datos para el perfil del cliente puede estar entre 2 y 30 minutos desde el momento de la recopilación de datos, y la disponibilidad para el lago de datos es de hasta 90 minutos.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Flujo de trabajo iniciado directamente por la interfaz de usuario.</li>
                    <li>Asignación de la interfaz de usuario para copiar props y eVars de Analytics en nuevos campos XDM.</li>
                    <li>La forma más rápida de obtener valor de Perfil del cliente en tiempo real y Customer Journey Analytics.</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">VÍNCULO PENDIENTE: Análisis con Customer Journey Analytics</a></td>
            <td>Experience Platform Edge</td>
            <td>
                <ul>
                    <li>Cuando se desea implementar una estrategia a largo plazo. Esto envía datos directamente desde un dispositivo al Experience Platform mediante el SDK web de AEP, el SDK móvil de AEP o la API del servidor de red perimetral.</li>
                    <li>Cuando sea un cliente nuevo o existente, que necesite disponibilidad de datos de Analytics en el perfil del cliente para admitir casos de uso de personalización de la misma página y de la siguiente.</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Proporciona el bueno nivel de control para los datos recopilados que se deben utilizar para admitir casos de uso.</li>
                    <li>Los datos del lado del cliente se asignan fácilmente a campos XDM.</li>
                    <li>La disponibilidad de datos más rápida para el Perfil del cliente en tiempo real.</li>
                </ul>
            </td>
        </tr>  
    </tbody>          
</table>
