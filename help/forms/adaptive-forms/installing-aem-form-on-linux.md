---
title: Instalación de AEM Forms en Linux
description: Aprenda a instalar bibliotecas de 32 bits para AEM Forms para la instalación de Linux.
feature: Adaptive Forms
type: Tutorial
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-7593
exl-id: b9809561-e9bd-4c67-bc18-5cab3e4aa138
last-substantial-update: 2019-06-09T00:00:00Z
duration: 204
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 0%

---

# Instalación de la versión de 32 bits de las bibliotecas compartidas

Cuando se implementa AEM FORMS OSGi o AEM Forms j2EE en Linux, debe asegurarse de que las versiones de 32 bits de un conjunto de bibliotecas compartidas estén instaladas y disponibles.  Las descripciones proceden de los propios paquetes.

* expat (Biblioteca C del analizador de XML orientado a secuencias para analizar XML, escrita por James Clark)
* fontconfig (Biblioteca de configuración y personalización de fuentes diseñada para localizar fuentes dentro del sistema y seleccionarlas según los requisitos especificados por las aplicaciones)
* freetype (Motor de renderización de fuentes, desarrollado para proporcionar compatibilidad avanzada con fuentes para una variedad de plataformas y entornos. Puede abrir y administrar archivos de fuentes, así como cargar, sugerir y procesar glifos individuales de forma eficaz. No es un servidor de fuentes o una biblioteca completa de renderización de texto)
* glibc (Bibliotecas principales para el sistema GNU y los sistemas GNU/Linux, así como muchos otros sistemas que usan Linux como núcleo)
* libcurl (biblioteca de transferencia de URL del lado del cliente)
* libICE (biblioteca de intercambio entre clientes)
* libicu (biblioteca que proporciona compatibilidad sólida y completa con Unicode y configuración regional: componentes internacionales para Unicode). Se requieren las ediciones de 64 y 32 bits de esta biblioteca
* libSM (biblioteca de administración de sesiones X11)
* libuuid (biblioteca de identificadores únicos universales compatible con DCE: se utiliza para generar identificadores únicos para objetos a los que se puede acceder más allá del sistema local)
* libX11 (biblioteca del lado del cliente X11)
* libXau (Protocolo de autorización X11: útil para restringir el acceso del cliente a la pantalla)
* libxcb (enlace de lenguaje C de protocolo X - XCB)
* libXext (Biblioteca para extensiones comunes del protocolo X11)
* libXinerama (extensión X11 que proporciona compatibilidad para extender un escritorio a través de varias pantallas. El nombre es un juego de palabras en Cinerama, un formato de película de pantalla ancha que utilizaba varios proyectores. libXinerama es la biblioteca que interactúa con la extensión RandR)
* libXrandr (la extensión de Xinerama es en gran medida obsoleta en la actualidad - ha sido reemplazada por la extensión RandR)
* libXrender (biblioteca de cliente de la extensión de procesamiento X)
nss-softokn-freebl (biblioteca Freebl para servicios de seguridad de red)
* zlib (biblioteca de compresión de datos de uso general, sin patente y sin pérdidas)

A partir de Red Hat Enterprise Linux 6, la edición de 32 bits de una biblioteca tendrá la extensión de nombre de archivo .686, mientras que la edición de 64 bits tendrá .x86_64. Por ejemplo, expat.i686. Antes de RHEL 6, las ediciones de 32 bits tenían la extensión .i386. Antes de instalar las ediciones de 32 bits, asegúrese de que están instaladas las últimas ediciones de 64 bits. Si la edición de 64 bits de una biblioteca es anterior a la versión de 32 bits que se está instalando, aparecerá un error como el siguiente:

0mError: Protected multilib versions: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: se encontraron problemas con la versión de Multilib.]

## Primera instalación

En Red Hat Enterprise Linux, utilice el modificador de actualización YellowDog (YUM) para instalar, como se muestra a continuación:

1. yum install expat.i686
2. yum install fontconfig.i686
3. yum install freetype.i686
4. yum install glibc.i686
5. yum install libcurl.i686
6. yum install libICE.i686
7. yum install libicu.i686
8. yum install libicu
9. yum install libSM.i686
10. yum install libuuid.i686
11. yum install libX11.i686
12. yum install libXau.i686
13. yum install libxcb.i686
14. yum install libXext.i686
15. yum install libXinerama.i686
16. yum install libXrandr.i686
17. yum install libXrender.i686
18. yum install nss-softokn-freebl.i686
19. yum install zlib.i686

## Enlaces simbólicos

Además, debe crear los enlaces simbólicos libcurl.so, libcrypto.so y libssl.so que apunten a las últimas versiones de 32 bits de las bibliotecas libcurl, libcrypto y libssl respectivamente. Puede encontrar los archivos en /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Actualizaciones en el sistema existente

Puede haber conflictos entre las arquitecturas x86_64 e i686 durante las actualizaciones, como este:
Error: Error de comprobación de transacción:
el archivo /lib/ld-2.28.so de la instalación de glibc-2.28-72.el8.i686 entra en conflicto con el archivo del paquete glibc32-2.28-42.1.el8.x86_64

Si se encuentra con esto, desinstale primero el paquete infractor, como en este caso:
yum remove glibc32-2.28-42.1.el8.x86_64

Dicho todo esto, desea que las versiones x86_64 e i686 sean exactamente iguales, como por ejemplo desde esta salida al comando:
información de yum glibc

Última comprobación de caducidad de metadatos: hace 0:41:33 el sábado 18 de enero de 2020 11:37:08 AM EST.
Paquetes instalados
Nombre: glibc
Versión : 2.28
Versión : 72.el8
Arquitectura : i686
Tamaño: 13 M
Source : glibc-2.28-72.el8.src.rpm
Repositorio : @System
Desde repositorio: BaseOS
Resumen : Las bibliotecas libc de GNU
URL : http://www.gnu.org/software/glibc/
Licencia: LGPLv2+ y LGPLv2+ con excepciones y GPLv2+ y GPLv2+ con excepciones y BSD e Inner-Net e ISC y Dominio Público y GFDL
Descripción : El paquete glibc contiene bibliotecas estándar que utilizan : varios programas del sistema. Para ahorrar espacio en el disco y : memoria, así como para facilitar la actualización, el código común del sistema es : guardado en un lugar y compartido entre programas. Este paquete en particular : contiene los conjuntos más importantes de bibliotecas compartidas: la biblioteca estándar C : y la biblioteca estándar matemática. Sin estas dos bibliotecas, un sistema : Linux no funcionará.

Nombre: glibc
Versión : 2.28
Versión : 72.el8
Arquitectura: x86_64
Tamaño: 15 M
Source : glibc-2.28-72.el8.src.rpm
Repositorio : @System
Desde repositorio: BaseOS
Resumen : Las bibliotecas libc de GNU
URL : http://www.gnu.org/software/glibc/
Licencia: LGPLv2+ y LGPLv2+ con excepciones y GPLv2+ y GPLv2+ con excepciones y BSD e Inner-Net e ISC y Dominio Público y GFDL
Descripción : El paquete glibc contiene bibliotecas estándar que utilizan : varios programas del sistema. Para ahorrar espacio en el disco y : memoria, así como para facilitar la actualización, el código común del sistema es : guardado en un lugar y compartido entre programas. Este paquete en particular : contiene los conjuntos más importantes de bibliotecas compartidas: la biblioteca estándar C : y la biblioteca estándar matemática. Sin estas dos bibliotecas, un sistema : Linux no funcionará.

## Algunos comandos yum útiles

lista yum instalada
búsqueda yum [part_of_package_name]
yum whatproviders [nombre_paquete]
yum install [nombre_paquete]
reinstalar yum [nombre_paquete]
información de yum [nombre_paquete]
yum deplist [nombre_paquete]
yum remove [nombre_paquete]
yum check-update [nombre_paquete]
actualización de yum [nombre_paquete]
