---
title: Instalación de AEM Forms en Linux.
description: La instalación de bibliotecas de 32 bits para AEM Forms funciona en la instalación de Linux.
feature: Formularios adaptables
audience: developer
doc-type: article
activity: setup
version: 6.4, 6.5
topic: desarrollo
role: Developer
level: Beginner
kt: 7593
translation-type: tm+mt
source-git-commit: da7837d45a9d5f614a4f6527b7bfe98aaf980d4f
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 0%

---


# Instalación de una versión de 32 bits de bibliotecas compartidas

Cuando AEM FORMS OSGi o AEM Forms j2EE se implementan en Linux, debe asegurarse de que las versiones de 32 bits de un conjunto de bibliotecas compartidas estén instaladas y disponibles.  Las descripciones son de los propios paquetes.

* expat (biblioteca C del analizador XML orientado a la emisión para analizar XML, escrita por James Clark)
* fontconfig (biblioteca de configuración y personalización de fuentes diseñada para localizar fuentes dentro del sistema y seleccionarlas según los requisitos especificados por las aplicaciones)
* freetype (motor de renderización de fuentes, desarrollado para proporcionar compatibilidad avanzada de fuentes para una variedad de plataformas y entornos. Puede abrir y administrar archivos de fuente, así como cargar, sugerir y procesar glifos individuales de forma eficaz. No es un servidor de fuentes ni una biblioteca completa de procesamiento de texto)
* glibc (Bibliotecas principales para sistemas GNU y GNU/Linux, así como muchos otros sistemas que usan Linux como núcleo)
* libcurl (biblioteca de transferencia de URL del lado del cliente)
* libICE (biblioteca de intercambio entre clientes)
* libicu (biblioteca que proporciona compatibilidad con Unicode y configuraciones regionales robustas y con todas las funciones - Componentes internacionales para Unicode). Se requieren las ediciones de 64 y 32 bits de esta biblioteca
* libSM (biblioteca de administración de sesiones X11)
* libuuid (biblioteca de identificador único universal compatible con DCE): se utiliza para generar identificadores únicos para objetos a los que se puede acceder más allá del sistema local.
* libX11 (biblioteca del lado del cliente X11)
* libXau (X11 Authorization Protocol - útil para restringir el acceso del cliente a la pantalla)
* libxcb (enlace en lenguaje C de protocolo X - XCB)
* libXext (Biblioteca para extensiones comunes del protocolo X11)
* libXinerama (extensión X11) que proporciona soporte para ampliar un escritorio en múltiples pantallas. El nombre es un juego de palabras en Cinerama, un formato de película de pantalla ancha que usa varios proyectores. libXinerama es la biblioteca que interactúa con la extensión RandR)
* libXrandr (la extensión de Xinerama es en gran medida obsoleta hoy en día - ha sido reemplazada por la extensión RandR)
* libXrender (biblioteca de cliente de extensión de procesamiento X)
nss-softokn-freebl (biblioteca Freebl para Servicios de seguridad de red)
* zlib (biblioteca de compresión de datos general, sin patente y sin pérdidas)

A partir de Red Hat Enterprise Linux 6, la edición de 32 bits de una biblioteca tendrá la extensión de nombre de archivo .686 mientras que la edición de 64 bits tendrá .x86_64. Ejemplo, expat.i686. Antes de RHEL 6, las ediciones de 32 bits tenían la extensión .i386. Antes de instalar las ediciones de 32 bits, asegúrese de que están instaladas las ediciones de 64 bits más recientes. Si la edición de 64 bits de una biblioteca es anterior a la versión de 32 bits que se está instalando, obtendrá un error como el siguiente:

0mError: Versiones de multilib protegidas: libsepol-2.5-10.el7.x86_64 != libsepol-2.5-6.el7.i686 [0mError: Se encontraron problemas con la versión de Multilib.]

## Primera vez que se instala

En Red Hat Enterprise Linux, use el Modificador de actualización de YellowDog (YUM) para instalar, como se muestra a continuación:

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

## Vínculos simbólicos

Además, debe crear enlaces simbólicos libcurl.so, libcrypto.so y libssl.so que apunten a las últimas versiones de 32 bits de las bibliotecas libcurl, libcrypto y libssl respectivamente. Puede encontrar los archivos en /usr/lib/
ln -s /usr/lib/libcurl.so.4.5.0 /usr/lib/libcurl.so
ln -s /usr/lib/libcrypto.so.1.1.1c /usr/lib/libcrypto.so
ln -s /usr/lib/libssl.so.1.1.1c /usr/lib/libssl.so

## Actualizaciones en el sistema existente

puede haber conflictos entre las arquitecturas x86_64 e i686 durante las actualizaciones, como por ejemplo:
Error: Error de comprobación de transacciones:
archivo /lib/ld-2.28.so desde la instalación de glibc-2.28-72.el8.i686 entra en conflicto con el archivo desde el paquete glibc32-2.28-42.1.el8.x86_64

Si se encuentra con esto, desinstale primero el paquete infractor, como en este caso:
eliminar glibc32-2.28-42.1.el8.x86_64

Todo dicho y hecho, usted desea que las versiones x86_64 e i686 sean exactamente iguales, como por ejemplo de esta salida al comando:
yum info glibc

Última comprobación de caducidad de metadatos: 0:41:33 hace el sábado 18 de enero de 2020 11:37:08 AM EST.
Paquetes instalados
Nombre : glibc
Versión : 2,28
Versión : 72.el8
Arquitectura : i686
Tamaño : 13 M
Fuente: glibc-2.28-72.el8.src.rpm
Repositorio : @System
Desde la cesión temporal: BaseOS
Resumen : Las bibliotecas libc de GNU
URL : http://www.gnu.org/software/glibc/
Licencia : LGPLv2+ y LGPLv2+ con excepciones y GPLv2+ y GPLv2+ con excepciones y BSD y Red interna e ISC y Dominio público y GFDL
Descripción : El paquete glibc contiene bibliotecas estándar que usa : varios programas del sistema. Para ahorrar espacio en disco y : , así como facilitar la actualización, el código común del sistema es : se mantienen en un lugar y se comparten entre los programas. Este paquete en particular : contiene los conjuntos más importantes de bibliotecas compartidas: la norma C : biblioteca y la biblioteca matemática estándar. Sin estas dos bibliotecas, un : El sistema Linux no funcionará.

Nombre : glibc
Versión : 2,28
Versión : 72.el8
Arquitectura : x86_64
Tamaño : 15 M
Fuente: glibc-2.28-72.el8.src.rpm
Repositorio : @System
Desde la cesión temporal: BaseOS
Resumen : Las bibliotecas libc de GNU
URL : http://www.gnu.org/software/glibc/
Licencia : LGPLv2+ y LGPLv2+ con excepciones y GPLv2+ y GPLv2+ con excepciones y BSD y Red interna e ISC y Dominio público y GFDL
Descripción : El paquete glibc contiene bibliotecas estándar que usa : varios programas del sistema. Para ahorrar espacio en disco y : , así como facilitar la actualización, el código común del sistema es : se mantienen en un lugar y se comparten entre los programas. Este paquete en particular : contiene los conjuntos más importantes de bibliotecas compartidas: la norma C : biblioteca y la biblioteca matemática estándar. Sin estas dos bibliotecas, un : El sistema Linux no funcionará.

## Algunos comandos útiles de yum

yum list instalado
yum search [part_of_package_name]
yum que proporciona [nombre_paquete]
yum install [package_name]
yum reinstalar [package_name]
yum info [package_name]
yum deplist [package_name]
eliminar [nombre_paquete]
yum check-update [package_name]
yum update [package_name]
