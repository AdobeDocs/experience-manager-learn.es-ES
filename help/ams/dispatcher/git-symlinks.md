---
title: Añadir enlaces de símbolos a GIT correctamente
description: Instrucciones sobre cómo y dónde añadir enlaces simbólicos al trabajar en las configuraciones de Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: df3afc60f765c18915eca3bb2d3556379383fafc
workflow-type: tm+mt
source-wordcount: '1252'
ht-degree: 0%

---


# Adición de enlaces simbólicos a GIT

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Comprobación de estado de Dispatcher](./health-check.md)

En AMS, obtendrá un repositorio GIT previamente rellenado que contenga el código fuente de Dispatcher maduro y listo para que inicie el desarrollo y la personalización.

Después de crear la primera `.vhost` archivo o nivel superior `farm.any` necesitará crear un vínculo simbólico desde el `available_*` para `enabled_*` directorio. El uso del tipo de vínculo adecuado será clave para una implementación correcta a través de la canalización de Cloud Manager. Esta página le ayudará a saber cómo hacerlo.

## Tipo de archivo de Dispatcher

El desarrollador de AEM inicia el proyecto normalmente desde el [AEM tipo de archivo](https://github.com/adobe/aem-project-archetype)

Este es un ejemplo del área del código fuente donde puede ver los enlaces simbólicos utilizados:

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

A modo de ejemplo, la variable `/etc/httpd/conf.d/available_vhosts/` contiene el potencial de ensayo `.vhost` archivos que podemos usar en nuestra configuración en ejecución.

La variable `.vhost` los archivos se mostrarán como una ruta relativa `symlinks` dentro del `/etc/httpd/conf.d/enabled_vhosts/` directorio.

## Creación de un enlace simbólico

Utilizamos enlaces simbólicos al archivo para que Apache Webserver trate el archivo de destino como el mismo archivo.  No queremos duplicar el archivo en ambos directorios.  En su lugar, sólo un acceso directo de un directorio (enlace simbólico) al otro.

Reconocer que las configuraciones implementadas se dirigirán a un host Linux.  La creación de un enlace simbólico que no sea compatible con el sistema de destino provocará errores y resultados no deseados.

Si su estación de trabajo no es una máquina Linux, probablemente se preguntará qué comandos utilizar para crear estos enlaces correctamente para que puedan enviarlos a GIT.

> `TIP:` Es importante utilizar vínculos relativos porque si instaló una copia local de Apache Webserver y tenía una base de instalación diferente, los enlaces seguirían funcionando.  Si utiliza una ruta absoluta, su estación de trabajo u otros sistemas tendrían que coincidir con la misma estructura de directorio exacta.

### OSX/Linux

Los enlaces simbólicos son nativos de estos sistemas operativos y aquí hay algunos ejemplos de cómo crear estos vínculos.  Abra la aplicación de terminal favorita y utilice los siguientes comandos de ejemplo para crear el vínculo:

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

Este es un ejemplo de comando rellenado para referencia:

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

Este es un ejemplo del vínculo si lista el archivo utilizando la variable `ls` comando:

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:` Resulta que MS Windows (mejor, NTFS) admite enlaces simbólicos desde... Windows Vista!

![Imagen del símbolo del sistema de Windows que muestra la salida de ayuda del comando mklink](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:` mklink para crear enlaces simbólicos requiere privilegios de administrador para ejecutarse correctamente. Incluso como cuenta de administrador, deberá ejecutar el símbolo del sistema &quot;Como administrador&quot; a menos que tenga activado el modo de desarrollador
> <br/>Permisos incorrectos:
> ![Imagen del símbolo del sistema de Windows que muestra el error del comando debido a los permisos](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>Permisos adecuados:
> ![Imagen del símbolo del sistema de Windows ejecutada como administrador](./assets/git-symlinks/windows-mklink-properpriv.png)

A continuación se muestran los comandos para crear el vínculo:

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


Este es un ejemplo de comando rellenado para referencia:

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### Modo de desarrollador ( Windows 10 )

Cuando se introduce [Modo de desarrollador](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), Windows 10 le permite probar aplicaciones más fácilmente, usar el entorno de shell de Ubuntu Bash, cambiar una variedad de configuraciones centradas en el desarrollador y hacer otras cosas por el estilo.

Microsoft parece seguir añadiendo funciones al modo de desarrollador o habilitando algunas de estas funciones de forma predeterminada una vez que alcanzan una adopción más generalizada y se consideran estables (por ejemplo, con la actualización de creadores, el entorno Ubuntu Bash Shell ya no necesita el modo de desarrollador).

¿Qué hay de los enlaces simbólicos? Con Developer Mode ENABLED, no es necesario ejecutar un símbolo del sistema con privilegios elevados para poder crear enlaces simbólicos. Por lo tanto, una vez activado el modo de desarrollador, cualquier usuario puede crear enlaces simbólicos.

> Después de habilitar el modo de desarrollador, los usuarios deben cerrar la sesión o iniciarla para que los cambios surtan efecto.

Ahora puede ver sin ejecutar como administrador el comando funciona

![Imagen del símbolo del sistema de Windows ejecutada como usuario normal con el Modo de desarrollador habilitado](./assets/git-symlinks/windows-mklink-devmode.png)

#### Enfoque alternativo/programático

Hay una política específica que permite a ciertos usuarios crear enlaces simbólicos → [Crear vínculos simbólicos (Windows 10) - Seguridad de Windows | Documentos de Microsoft](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

PRO:
- Los clientes podrían aprovechar esto para permitir mediante programación la creación de vínculos simbólicos a todos los desarrolladores de su organización (es decir, Active Directory) sin tener que habilitar el Modo de desarrollador manualmente en cada dispositivo.
- Además, esta directiva debería estar disponible en versiones anteriores de MS Windows que no ofrecen el Modo de desarrollador.

CON:
- Esta directiva parece no tener ningún efecto en los usuarios que pertenecen al grupo Administradores. Los administradores seguirían necesitando ejecutar el símbolo del sistema con privilegios elevados. Extraño.

> Para que los cambios en la directiva local/de grupo surtan efecto, será necesario cerrar sesión/iniciar sesión del usuario.

Ejecutar `gpedit.msc`, agregue o cambie usuarios según sea necesario. Los administradores están allí de forma predeterminada

![Ventana Editor de directivas de grupo que muestra el permiso para ajustar](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### Habilitar los enlaces simbólicos en GIT

Git gestiona los enlaces simbólicos según la opción core.symlinks

Fuente: [Git: documentación de git-config](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*Si core.symlinks es false, los vínculos simbólicos se extraen como pequeños archivos sin formato que contienen el texto del vínculo. `git-update-index[1]` y `git-add[1]` no cambiará el tipo registrado a archivo normal. Útil en sistemas de ficheros como FAT que no admiten enlaces simbólicos.
El valor predeterminado es true, excepto `git-clone[1]` o `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.` En la mayoría de los casos, Git supondrá que Windows no es bueno para los enlaces simbólicos y lo establecerá en falso.*

El comportamiento de Git en Windows se explica bien aquí: Vínculos simbólicos ・ wiki de git-for-windows/git ・ GitHub

> `Info`: Las suposiciones enumeradas en la documentación relacionada arriba parecen estar bien con una posible configuración AEM Developer&#39;s en Windows, especialmente NTFS y el hecho de que solo tenemos enlaces simbólicos de archivos vs. enlaces simbólicos de directorios

Estas son las buenas noticias, ya que [Git para Windows versión 2.10.2](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1) el instalador tiene un [opción explícita para activar la compatibilidad con enlaces simbólicos.](https://github.com/git-for-windows/git/issues/921)

> `Warning`: La opción core.symlink se puede proporcionar durante la ejecución mientras se clona el repositorio, o bien se puede almacenar como una configuración global.

![Muestra del instalador GIT que muestra la compatibilidad con enlaces simbólicos](./assets/git-symlinks/windows-git-install-symlink.png)

Git para Windows almacenará las preferencias globales en `"C:\Program Files\Git\etc\gitconfig"` . Es posible que otras aplicaciones cliente de escritorio de Git no consideren estos ajustes.
Esta es la captura, no todos los desarrolladores utilizarán el cliente nativo de Git (por ejemplo, Git Cmd, Git Bash) y algunas de las aplicaciones de escritorio de Git (por ejemplo, GitHub Desktop, Atlassian Sourcetree) pueden tener diferentes configuraciones/valores predeterminados para usar System o un Git integrado

Aquí hay una muestra de lo que hay dentro del `gitconfig` file

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Sugerencias de la línea de comandos de Git

Puede haber escenarios en los que tenga que crear nuevos vínculos simbólicos (por ejemplo, agregar un nuevo host o una nueva granja).

Hemos visto en la documentación anterior que Windows ofrece un comando &quot;mklink&quot; para crear vínculos simbólicos.

Si trabaja en un entorno de Git Bash, puede usar el comando estándar Bash `ln -s` pero tendrá que ir precedido de una instrucción especial como en el ejemplo siguiente:

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### Resumen

Para que Git gestione los enlaces simbólicos correctamente (al menos para el ámbito de la línea de base de configuración actual de Dispatcher AEM) en un sistema operativo Microsoft Windows, necesitará:

| Elemento | Versión mínima/configuración | Versión y configuración recomendadas |
|------|---------------------------------|-------------------------------------|
| Sistema operativo | Windows Vista o posterior | Windows 10 Creator Update o posterior |
| Sistema de archivos | NTFS | NTFS |
| Capacidad de gestionar enlaces simbólicos para el usuario de Windows | `"Create symbolic links"` grupo/política local `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Modo de desarrollador de Windows 10 habilitado |
| GIT | Versión de cliente nativa 1.5.3 | Cliente nativo versión 2.10.2 o posterior |
| Configuración de Git | `--core.symlinks=true` opción al hacer un clon de git desde la línea de comandos | Configuración global de Git<br/>`[core]`<br/>    symlinks = true <br/> Ruta de configuración del cliente Git nativo: `C:\Program Files\Git\etc\gitconfig` <br/>Ubicación estándar para clientes de Git Desktop: `%HOMEPATH%\.gitconfig` |

> `Note:` Si ya tiene un repositorio local, tendrá que clonar de nuevo desde el origen. Puede clonar en una nueva ubicación y combinar manualmente los cambios locales no comprometidos/no organizados en el repositorio recién clonado.