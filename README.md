# Respuestas LFS 2020
### Josue Valenzuela 171001

**Revise el código de `version-check.sh` para responder lo siguiente:**



*   ¿Por qué las condiciones de los if’s están entre corchetes?

    Los corchetes son referencias al comando `test`

*   ¿Cuáles son las condiciones que se están verificando en cada caso?

    `-h` verifica que el archivo exista y sea un *link* simbólico


    `-x` verifica que el archivo exista y tenga permiso para ser ejecutado.

*   ¿Para qué sirve `$` en shellcode?

    Tiene varios usos:

*   Para expansión de variables: Si se usa como prefijo a una variable se extrae el valor de la variable.
*   Fin de línea: En una expresión regular indica el fin de una línea
*   Al usarse de la forma ${} es expansión de variables.

**Vea el contenido de INSTALL en uno de los paquetes listados como requerimientos para el LFS**

INSTALL en `autoconf-2.69`



*   ¿Cuáles son los tres comandos que se deben ejecutar para instalar un paquete en Linux?
1. `./configuer`
2. `make`
3. `make install`

	Configuran, construyen  e instalan el paquete



*   ¿Dónde encontramos información adicional de instalación específica para cada paquete?

    En el  archivo `README` que es incluido con cada paquete.


**Parte del proceso de instalación involucra descargar y extraer las *tarballs* de las herramientas.**



*   ¿Qué es una *tarball*?

    Es una colección de archivos que son almacenados dentro de un solo archivo con la extensión `.tar`, almacena la información del archivo, permisos, dueños, *etc*.

*   ¿Por qué las *tarball* tienen una extensión “doble” (e.g., `.tar.gz`)?

    Esto indica que la `tarball` ha sido comprimido, por ejemplo, una *tarball* con extensión `.tar.gz` es una *tarball* que ha sido comprimido utilizando *gzip*


**La extracción de una *tarball* suele hacerse con el comando `tar -xvzf` seguido del archivo que se desea descomprimir.**



*   ¿Qué significa cada una de las opciones (`-xvzf`)?

	



*   `-x` Indica que deben de extraerse archivos de una *tarball*
*   `-v` Modo ‘verbose’, lista los archivos que han sido extraídos
*   `-z` Indica que debe de pasar por el filtro de gzip (comprimir)
*   `-f` Indica el nombre de la *tarball* es el siguiente bloque
*   ¿Cuál de estas opciones podría haberse omitido?

	

	La opción `-v` pueden omitirse, solo indica que deben listarse en pantalla los archivos que son procesados



*   ¿Cuál de estas opciones cambia si la extensión del archivo comprimido es diferente a .tar.gz?

	La opción `-z` debe de cambiarse si la extensión del archivo es diferente a `.tar.gz`. Por ejemplo si la extensión de la *tarball* es `.tar.xz` la opción `-z` debe de reemplazarse por `-J`

**Enlaces simbólicos**



*   ¿Qué es un enlace simbólico?

	Es una referencia a otro archivo, no contiene información del archivo si no que solo la referencia a este archivo, parecido a un atajo.

	



*   ¿Cuál es la diferencia entre un enlace simbólico (*symlink*) y uno duro (*hard link*)?

	Un *hardlink* contiene la referencia al `inode` del archivo, mientras que un *symlink* contiene la referencia al nombre de un archivo. Esto quiere decir que si un *symlink* es eliminado o renombrado este no afectará el archivo original por que contiene solo la referencia a ese archivo, mientras que un *hardlink* si se realizan cambios afectan al `inode` lo que se verá reflejado en todos los archivos que apunten a ese `inode`

	



*   En el comando `ln` usado para crear enlaces, ¿en qué orden se deben escribir los parámetros para crear un enlace de A a B?

    Para crear un *link* de A (TARGET) a B (Nombre de *LINK*) se hace de la forma:

    ```
    ln A B
    ```


**Preparación de un ambiente aislado para creación de un LFS**

Esto  involucra crear un  archivo llamado `.bash_profile`



*   ¿Qué hacen los archivos `.bash_profile`?

    Es un archivo de configuración para que es ejecutado cada vez que una nueva sesión de `bash` es iniciada.

*   ¿Para qué sirve crear un archivo llamado `.profile`?

    El archivo `.profile` es el archivo de configuración que era utilizado en  *Bourne shell*. *Bash* al ser compatible con  también ejecuta *Bourne shell* dicho archivo. La ventaja de crear dicho archivo es que todas las implementaciones de un shell compatible con *Bourne shell* van a ejecutar dicho archivo.


La  creación  del  ambiente  aislado  involucra  también  crear  un  usuario  llamado lfs con  el  siguiente comando:

```bash
sudo useradd –s /bin/bash –g lfs –m –k /dev/null lfs
```



*   ¿Qué es `/dev/null`?

	Es un archivo especial en *unix* llamado `null devices`. Todo lo que es escrito a este archivo es descartado inmediatamente, cuando es leído solamente retorna el final del archivo (`EOF`)



*   Explique las opciones y parámetros usados en este comando:
*   `-s /bin/bash` configura `bash` como el *shell* predeterminado para el usuario *lfs*
*   `-g lfs` agrega el usuario *lfs* al grupo *lfs*
*   `-m` crea el directorio *home* para el usuario *lfs*
*   `-k /dev/null` previene que se copien archivos dando como destino *null devices*
*   `lfs` es el nombre del grupo y usuario que se crea

Se le otorga a este usuario la propiedad de algunos directorios con el comando `chown`



*   ¿Qué significa ser dueño de un directorio?

    Linux al ser un sistema multiusuario define ser dueño de un archivo para definir los permisos que se tienen sobre dicho archivo. El dueño del archivo puede definir los permisos para otros usuarios o grupos de usuarios sobre el archivo.

*   ¿Cuál es la diferencia entre una *login shell* y una *non-login shell*?

	

	Una *login shell* es aquella cuyo primer parámetro es `-`, cuando se ejecuta una *login shell* se busca y ejecuta (si existen) los archivos ` /etc/profile`, `~/.bash_profile`, `~/.bash_login`, y `~/.profile` en este orden; y al salir se busca y ejecuta (si existen) los archivos  `~/.bash_logout` y `/etc/bash.bash_logout`. Cuando se ejecuta una *non-login shell* se busca y ejecuta (si existe) el archivo `~/.bashrc`.

El archivo `.bash_profile` que contendrá el siguiente comando:

```bash
exec env –i HOME=$HOME TERM=$TERM PS1='\u:\w\$ ' /bin/bash
```



*   Explique  exactamente  qué  se  está  haciendo  con `env –i  HOME=$HOME  TERM=$TERM PS1='\u:\w\$ ' /bin/bash`

    El comando reemplaza la *shell* actual con una nueva en un ambiente vacío, manteniendo las solamente las variables `HOME`, `TERM,` y `PS1` 

*   ¿Cuál es el efecto o propósito de preceder el comando anterior con exec?

    Reemplazar la *shell* sin crear un nuevo proceso.


El archivo `.bashrc` contiene las líneas `set +h` y `umask 022`. `set +h` apaga la funcionalidad de *hashing* para la *shel*l.



*   ¿Para qué sirve el *hashing* de la *shell*?

    *bash* usa una *hash table* para almacenar la ruta de un ejecutable y evitar tener que buscar `PATH` cada vez que un ejecutable es invocado.

*   ¿Qué hace el comando `umask` y cuál es el efecto de usarlo con el parámetro `022`?

    `umask` define la máscara de permisos en la creación de los archivos, el parámetro `022` define los permisos para que solo puedan ser escritos por el dueño del archivo pero ejecutados por cualquier otro usuario


**Instalación de toolchain para creación de un LFS**

**Primera instalación de Binutils**



*   ¿Qué   se   logra   (más   allá   de   los   efectos   en   la   configuración)   al   especificar `--with-sysroot=$LFS`?

    Para *cross compilation* le dice al sistema que debe de buscar las librerías del sistema objetivo necesarias en `$LFS`

*   ¿Por qué se especifica `$LFS_TGT` como el valor de `--target`?

    Debido a que el valor de la variable `$LFS_TGT` es distinto al valor retornado por `config.guess` ajusta `configure` para construir un `cross linker`

*   ¿Qué es la *target triplet* y cuál es su campo *vendor*?

    *target triplet* es el nombre de plataforma en la que se está trabajando, usualmente tienen el formato `machine-vendor-operatingsystem` pero el campo `vendor` puede ser omitido.

*   ¿Cuál es el significado o propósito del campo *vendor*, y para qué sirve le asignamos el valor *lfs*?

    Usualmente el valor es irrelevante, pero cuando es definido es para definir que el paquete funciona para dicho *vendor*, en caso de definir el valor *lfs* es para indicar que los paquetes están compilados para que funciones con *lfs*.

*   ¿Cómo se relaciona la *target triplet* con la opción `--prefix=/tools`, también usada?

    La opción `--prefix=/tools` indica que se instalen los paquetes en el directorio `/tools` que en este caso es el directorio de nuestro lfs.


**Primera instalación de GCC**

```bash
for file in gcc/config/{linux,i386/linux{,64}}.h

do

  cp -uv $file{,.orig}

  sed -e 's@/lib\(64\)\?\(32\)\?/ld@/tools&@g' \

  	-e 's@/usr@/tools@g' $file.orig > $file

  echo '
    #undef STANDARD_STARTFILE_PREFIX_1

    #undef STANDARD_STARTFILE_PREFIX_2

    #define STANDARD_STARTFILE_PREFIX_1 "/tools/lib/"

    #define STANDARD_STARTFILE_PREFIX_2 ""' >> $file

  touch $file.orig

done
```
*   ¿Para qué sirve encerrar una lista de strings entre llaves, en este caso?

    Esto es conocido como *brace expansion* es utilizado para expandir los parámetros entre ‘{’ y ‘}’ para generar distintos *strings*. Por ejemplo la expansión `gcc/config/{linux,i386/linux{,64}}.h` genera los strings  gcc/config/linux.h, gcc/config/i386/linux.h, y gcc/config/i368/linux64.h

*   ¿Qué hace el programa *sed*? Aclare con el mayor detalle posible qué está haciendo sed con ese relajo abajo de cp –uv $file{,.orig}.

    *sed* es un editor de *streams* se utiliza para realizar transformaciones de texto. La primera expresión de *sed* agrega el prefijo “/tools” a toda aparición de “/lib/ld”, “/lib64/ld” o “/lib32/ld.La segunda expresión de *sed* reemplaza ‘/usr’.

*   ¿Qué hace la instrucción touch?

    Actualiza la marca de tiempo en los archivos.

*   ¿Qué hace la opción `–u` de `cp` y cómo se relaciona con `touch` en este ciclo?

    Asegura que el archivo se copie si la fuente es más reciente que el destino, o el destino no existe. `touch` al actualizar la marca de tiempo del archivo se asegura que no hayan cambios inesperados en caso que se corran los comandos mas de una vez.

*   ¿Para qué  sirven las macros STANDARD_STARTFILE_PREFIX_N y por qué se  les asignan los valores que se les asignan en este ciclo?

    Sobreescriben los valores predeterminados que se utilizan para buscar librerías dentro del sistema (`/lib`, `libdir`). Se definen los valores para que las librerías sean buscadas en el directorio de librerías para el lfs.


**En la instrucción de configuración para GCC:**

```bash
../configure                                       \

    --target=$LFS_TGT                              \

    --prefix=/tools                                \

    --with-glibc-version=2.11                      \

    --with-sysroot=$LFS                            \

    --with-newlib                                  \

    --without-headers                              \

    --with-local-prefix=/tools                     \

    --with-native-system-header-dir=/tools/include \

    --disable-nls                                  \

    --disable-shared                               \

    --disable-multilib                             \

    --disable-decimal-float                        \

    --disable-threads                              \

    --disable-libatomic                            \

    --disable-libgomp                              \

    --disable-libquadmath                          \

    --disable-libssp                               \

    --disable-libvtv                               \

    --disable-libstdcxx                            \

    --enable-languages=c,c++
```

*   ¿Qué hace la opción `--disable-nls`?

    Desactiva el soporte para lenguaje nativo (NATIVE LANGUAGE SUPPORT)

*   ¿Qué hace la opción `--disable-shared` y por qué es necesaria? ¿Cuál es la diferencia entre *linking* estático y dinámico?

    Fuerza a *GCC* a hacer *linki* estático a las librerías internas. *Linking* estático fuerza hacer *link* con las librerías al momento de compilación (compile time), mientras que *linking* dinámico ejecuta el *link* al momento que se corre (run time).


**Instalación de Glibc**



*   ¿Qué hace la ejecución del script `config.guess` en `glibc-2.30/scripts`?

    Retorna el *target triplet* de la máquina en donde se está trabajando.

*   ¿Cuál es el efecto de las opciones `--host`y `--build` en la configuración?

    Causa que *glibc* se configure para que utilice *cross-compile*, con el *cross-linker* y *cross-compiler* en el directorio  `/tools` del lfs.


**Sanity check**

**Para ello se usa la herramienta de *Binutils* `readelf`.**



*   ¿Qué es un archivo *ELF* y qué hace la opción `–l` de `readelf`?

    ELF (Executable and Linkable Format)  especifica el formato para archivos binarios, librerías y *core files*. La opción ‘-l’ de `readelf` muestra la información en el *header* del archivo ELF


**Hasta el momento se han instalado GCC y Binutils con configuraciones que conforman lo que el libro llama cross GCC y cross Binutils. Luego de esto instalará el paquete Libstdc++.**



*   ¿Qué es un *cross compiler* y por qué lo necesitamos? Se recomienda leer la sección 5.2 del libro de LFS.

    *cross compiler* se utiliza para compilar como objetivo a una plataforma distinta a donde se está ejecutando el compilador.

*   ¿Cuál es el efecto de compilar con `$LFS_TGT-gcc` en lugar de sólo `gcc`?

    Asegura que se utilice el *cross compiler* y sus herramientas del lfs en lugar de las del *host*

*   Explique la relación entre *Binutils, GCC, Glibc y Libstdc++.*

    GCC es la colección de compiladores de *GNU*, *Glibc* es la implementación de *c* de *GNU*, *Libstdc++* es la implementación de *c++* de *GNU*, y *Binutils* son herramientas para traducir ensamblador a código objeto, en conjunto se utilizan para compilar *c* y *c++*

*   ¿Por qué se usa la opción `--with-gxx-include-dir=/tools/$LFS_TGT/include/c++/9.2.0` en la configuración?

    Para indicar al compilador de c++ que se busquen los archivos estándar en *include* en el directorio del lfs.


**Segunda instalación de Binutils**



*   ¿Por qué se debe realizar una segunda instalación de Binutils?

    Para poder modificar las configuraciones del compilador que se utiliza (*cross compiler* o compilador del host).

*   ¿Qué hacen las herramientas `ranlib` y `ar`, de *Binutils*?
    *   `ar` crea modifica y extrae de archivos
    *   `ranlib` crea un índice del contenido de un archivo  (archive) y lo almacena dentro del archivo (archive)

**Segunda instalación de GCC**



*   ¿Para qué sirve el comando `dirname`?

    Transforma un string con terminador `null` que contiene la ruta de un archivo en componentes de directorios y archivos.

*   ¿Cuál es la diferencia entre ejecutar algo con `$` y ejecutarlo con ```?

    Cuando se ejecuta un comando con ``` este hace que el resultado esté disponible para ser almacenado dentro de una variable. Ejecutarlo con `$` es expansión de comandos, donde se ejecuta el comando y el resultado es retornado utilizando `pipe` al contexto original.

*   ¿Qué contiene el *header* `limits.h`?

     Contiene los límites de valores de los tipos integrales esenciales para c/c++. SOn específicos para el sistema, y para la implementación del compilador.

*   Normalmente GCC ejecuta este mismo comando durante su instalación y el resultado es un *header* llamado `limits.h` que incluye otro *header* llamado `limits.h` provisto por el sistema. ¿Por qué la primera instalación de GCC no hizo esto desde un principio?

    Al momento de la primera instalación de `gcc` el archivo `/tools/include/limits.h` no existía, por lo que no podía ser incluido en el *header*, esto es debido a que no se tenían todas las dependencias necesarias al momento de la primera instalación.


**El comando de configuración de GCC que se usará emplea la opción `--disable-bootstrap` que deshabilita el comportamiento habitual que tiene GCC de compilarse haciendo un *bootstrap build*.**



*   ¿Qué es un *bootstrap build*?

    Esto significa que la compilación se lleva a cabo más de una vez, gcc al terminar de compilar la primera vez utiliza el resultado para volverse a compilar una segunda vez, el resultado es de nuevo utilizado para compilarse una tercera vez. El resultado entre cada una de las etapas es comparado para asegurarse que puede reproducirse a sí mismo.


**Instalará un paquete llamado Tcl, otro llamado Expecty otro más llamado DejaGNU**



*   ¿Qué es Tcl y con qué propósito fue creado?

    tcl (Tool Command Language) es un lenguaje de *scripting* de propósito general, tipo dinámico, fue creado originalmente para ser un lenguaje de comandos que sea fácil de reutilizar y aprender. 

*   ¿Para qué sirve Expect?

    Es utilizado para comunicarse con un programa interactivo en base a un *script*


    [https://linux.die.net/man/1/expect](https://linux.die.net/man/1/expect)

*   ¿Cuál es la relación entre DejaGNU, Expect y Tcl?

    DejaGNU está escrito utilizando Expect, TCL es el lenguaje que se utilizao para correr los tests de DejaGNU


**Instalará otros paquetes llamados: M4, Ncurses, Bash,Coreutils,Diffutils,Gettext, Make y Patch. En las opciones de configuración de Bash se especificará `--without-bash-malloc`. Durante la instalación del  paquete  Gettext será  necesario  que  compile  los  programas msgfmt, msgmerge y xgettext.  Y para la configuración de Make usará la opción `--without-guile`.**



*   ¿Para qué sirve Ncurses?

    Es un emulador de *curses* para *System V*. Es utilizado para realizar interfaces gráficas (GUI) que corren en un emulador de terminal.

*   ¿Qué es `malloc`?

    `malloc` es la función para asignación de memoria (memory allocation) recibe el tamano de memoria que se desea asignar (en bytes) y retorna un puntero a la memoria asignada.

*   ¿Para qué sirve `M4`?

    Es un procesador de *macros*, esto quiere decir que expande *macors* definidos por el usuario en el *output* completo que fue definido para dicho macro.

*   ¿Qué hace el programa `host name` de Coreutils?

    Se usa para imprimir el nombre de la máquina *host* donde esta corriendo.

*   ¿Cuál es la relación entre Patch y Diffutils?

    Patch es una opción incluida en *diffutils* utilizada para reemplazar un archivo original con la versión que contiene el *patch*

*   ¿Cuál es la relación entre los programas `msgfmt`, `msgmerge` y` xgettext` de `Gettext`?

    `msgfmt` se usa para genera un catálogo de mensajes binarios a partir de un catálogo de traducción, `msgmerge` se usa para combinar dos traducciones `raw` en un solo archivo, y `xgettext` se usa para extraer las líneas de mensaje traducibles de los archivos fuente dados para hacer la primera plantilla de traducción.[t](http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/gettext.html#contents-gettext)

*   ¿Qué es `GNU Guile`?

    Es una implementación del lenguaje de programación *scheme*. GUILE es lenguaje de extensión por defecto del proyecto GNU.

*   Investigue y explique qué son las *automatic variables* en un makefile.

    Son variables cuyos valores son computados cada vez que una *regla* es ejecutada.


**Para  la  remoción  de debugging symbols se usan los siguientes comandos**:

**Pasos no se realizaron al ser opcionales y contar con suficiente espacio**

```bash
strip --strip-debug /tools/lib/*

/usr/bin/strip --strip-unneeded /tools/{,s}bin/*

rm –rf /tools/{,share}/{info,man,doc}
````



*   ¿Qué son *debugging symbols*?

    Son símbolos que almacenan información adicional (variable, función *etc*) a la *tabla simbólica* de un objeto para facilitar realizar *debug*.

*   ¿Para qué sirve `strip`?

    Descarta todos los símbolos de los archivos de objetos objfile. La lista de archivos de objetos puede incluir archivos

*   ¿Por   qué   el   primer   comando   simplemente   ejecuta `strip` pero   el   segundo   ejecuta `/usr/bin/strip`?

    Se utiliza `strip` del sistema ( `/usr/bin/strip`) para poder incluir `strip` en la carpeta `/tools` del lfs.

*   Explique el concepto de *relocation* asociado a los *linkers*, y su relación con la opción `--strip-unneeded`.

    Es el proceso para asignar direcciones de carga a diferentes partes del programa, donde después son fusionadas en una sola. `--strip-unneeded`. indica que los linkers van a ser removidos de no ser necesarios.


**Construcción de LFS**

**Montar sistemas de archivos**



*   ¿Qué es un sistema de archivos?

    Un sistema de archivos, es el conjunto de métodos, estructuras de datos que se utilizan para organizar los archivos que se almacenan en un espacio de memoria.


    ¿Qué es un sistema de archivos virtual?


    Un sistema de archivos virtual (VFS) es una abstracción de un sistema de archivos, tienen 2 funciones principales:

1. Separar las operaciones genéricas (de un sistema de archivos) de su implementación. Se logra definiendo una interfaz VFS.
2. Provee un mecanismo de representación única para un archivo a través de una red.

	(Silberschatz, A, _et al, _2012)



*   ¿Qué significa “montar” un sistema de archivos?

    En sistemas *unix* los archivos que son accesibles son almacenados en un árbol, cuya raíz se encuentra en `\`. Muchas veces los archivos se encuentran en distintos dispositivos (o en el mismo), por consiguiente distintos sistemas de archivos. Montar un sistema de archivos se refiere a adjuntar el sistema de archivos al árbol de archivos del *host* para que puedan ser accedidos.

*   Explique el propósito de cada uno de los sistemas de archivos `/dev`, `/proc`, `/sysfs` y `/run`.
    *   `/dev` contiene todos los archivos especiales que representan a los dispositivos. 
    *   `/proc` contiene un sistema de archivos *illusionary*, este no existe en almacenamiento (secundario) si no que es creado por el kernel en memoria (primaria), se utiliza para proveer información del sistema. 
    *   `/sysfs` es un pseudo sistema de archivos que se usa para proveer una interfaz a las estructuras de datos del kernel. 
    *   `/run` se usa para almacenar información que describe el sistema desde que fue iniciado.

 



*   Explique a detalle qué hacen los comandos siguientes:

    ```bash
    sudo mknod –m 600 $LFS/dev/console c 5 1

    sudo mknod–m 666 $LFS/dev/null c 1 3
    ```


    `mknod` crea un archivo especial (nodo en el sistema de archivos); la opción `-m` se utiliza para determinar los permisos del archivo a crear. 

*   Modo `600` define permisos al usuario de lectura y escritura, pero no otorga permisos para grupo u otros (rw-------)
*   Modo `666` define permisos de lectura y escritura para usuario, grupo, y otros (rw-rw-rw-)

	El tipo de archivo `c` es un archivo especial de carácter (unbuffered). Los últimos 2 números que pasan como parámetro son *major* y *minor*, estos representan que *driver* debe de utilizarse para el manejo del archivo.

	



*   Explique a detalle qué hacen los comandos `mount` siguientes:

    ```bash
    sudo mount –vt devpts devpts $LFS/dev/pts –o gid=5,mode=620


    sudo mount –vt proc proc $LFS/proc


    sudo mount –vt sysfs sysfs $LFS/sys


    sudo mount –vt tmpfs tmpfs $LFS/run
    ```

*   Para acceder a un ambiente temporal se usará un comando como el siguiente:

    ```bash
    sudo chroot “$LFS” /tools/bin/env –i \


    HOME=/root \


        TERM=”$TERM” \


        PS1=’(lfs chroot) \u:\w\$ ‘ \


        PATH=/bin:/usr/bin:/sbin:/usr/sbin:/tools/bin \


        /tools/bin/bash --login +h
    ```


    Explique  exactamente  qué hace  el comando anterior y por qué es  importante para el resto del proyecto.

*   `-i` indica que las variables serán *limpiadas* en el nuevo entorno.
*   `HOME`, `TERM`, `PS1`, `PATH` son configuradas dentro del ambiente `chroot`
*   `/tools/bin/bash --login +h` activa el ambiente sin las herramientas temporales y deshabilitando *hashing*
*   ¿Por  qué  es  necesario  crear un conjunto  de *links* simbólicos  que  apuntan a  directorios  como `/bin`, `/usr/bin` y `/usr/liba/tools/bin` y `/tools/lib`, correspondientemente?

    Muchos de los *programas* que conforman el *toolchain* requieren que muchas dependencias se encuentren en directorios específicos, donde no están instalados en nuestro ambiente. Para satisfacer dichos requisitos se crean los *links*

*   ¿Para qué sirve el comando `install`?

    Se usa para copiar archivos al destino especificado, también se encarga en definir los permisos cuando los archivos son copiados.

*   ¿Para qué sirve el archivo `/etc/mtab` y por qué se *linkea* a `/proc/self/mounts`?

    `/etc/mtab` contiene una lista de los sistemas de archivos que se encuentran montados actualmente. Las versiones más recientes del kernel utilizan el sistema de archivos `/proc` para lograr el mismo objetivo. El *link* se hace para mantener la compatibilidad con software que espere el directorio `/etc/mtab`.

*   ¿Qué es el *Filesystem Hierarchy Standard (FHS)*?

    Es un estándar que define los requerimientos y pautas para la colocación de archivos y directorios en sistema operativos *Unix like*


**.Luego de crearse el archivo `/etc/passwd` se ejecutará el siguiente comando:**

```
exec /tools/bin/bash --login +h
```



*   ¿Para qué sirven los archivos `/etc/passwd` y `/etc/group`? Explique la sintaxis del contenido de estos archivos
*   `/etc/passwd` es la base de datos de usuarios del sistema. Contiene el nombre de usuario, nombre real, directorio `home`.

    Dentro del archivo contiene la información del usuario por línea en el formato:

        name:password:UID:GID:GECOS:directory:shell
        

*   `/etc/group` similar a `/etc/passwd` pero describe grupos en lugar de usuarios.

    Dentro del archivo contiene la información del grupo por línea en el formato:

        group_name:password:GID:user_list

*   Explique el propósito y configuración de los usuarios creados en `/etc/passwd`.

    Su propósito es definir las contraseñas que se van a utilizar.

*   ¿Cuál  es  el propósito de  ejecutar  el  comando `exec` mostrado  arriba,  y  por  qué  se logra  ese propósito usando` exe`c en lugar de sólo `/tools/bin/bash`?

    Se utiliza para que una nueva shell sea creada, de lo contrario la nueva shell seria una subshell de la actual.


**Instalación de Glibc**

Se comienza por aplicar un parche al paquete con el siguiente comando:

```
patch -Np1 -i ../glibc-2.30-fhs-1.patch
```



*   Dé un vistazo al contenido del archivo `glibc-2.30-fhs-1.patch` y responda: ¿qué hace el programa `patch`?

    Aplica cambios a un archivo, que entran con el formato de `diff`. Similar a como `git` muestra las diferencias entre archivos.

*   ¿Para qué sirven las opciones `–Np1` e `–i` especificadas?
*   `-N` ignora los `patch` que parecen ya estar aplicados.
*   `-p1` Elimina el prefijo más pequeño que contiene 1 diagonal.
*   `-i` se usa para indicar el archivo `patch`

	



*   ¿Qué hace `touch` cuando recibe un archivo inexistente como argumento?

    Crea un archivo vacío (a menos que las opciones `-c` o `-h` se apliquen).

*   ¿Qué es `nscd`?

    Es un *daemon* que provee *cache* a los nombres de las solicitudes de servicio más comunes.

*   ¿Cuál es el propósito de `nsswitch.conf`?

    Es el archivo de configuración de `nscd`. Cada linea especifica los atributos, nombre y servicios.

*   ¿Cuál es el propósito de cada una de las bases de datos configuradas en este archivo?

    Define el comportamiento de `nscd`. Dependiendo del contenido del archivo determina el funcionamiento de nscd`.

*   ¿Para qué sirve el comando `zic`?

    Se encarga de compilar los archivos que se le pasan y genera los archivos de información de conversión de tiempo.

*   ¿Cuál es el propósito de los archivos `zone.tab`, `zone1970.tab` e `iso3166.tab`?
*   `zone.tab` es una tabla que contiene la informacion de tiempo de cada pais.
*    `zone1970.tab` contienen la informacion de tiempo de 1970 (Se utiliza como base para los cálculos de tiempo actuales)
*   `iso3166.tab` contiene la informacion de tiempo siguiendo el formato iso3166.

**Se modificarán las especificaciones  de  GCC  para que se adapten al uso de este nuevo loader,con el siguiente comando:**

```bash
gcc -dumpspecs | sed -e 's@/tools@@g'               	\

	-e '/\*startfile_prefix_spec:/{n;s@.*@/usr/lib/ @}' \

	-e '/\*cpp:/{n;s@$@ -isystem /usr/include@}' >  	\

	`dirname $(gcc --print-libgcc-file-name)`/specs
```



*   ¿Qué hace la instrucción `n` en `sed`?
*   ¿Cuál es el propósito del archivo `specs`?

**Sanity check**

Se compila un archivo de prueba con las siguientes instrucciones:

```bash
cc dummy.c –v –Wl,--verbose &> dummy.log
```



*   ¿Qué hace la opción `–Wl` en el comando `cc`?

    Pasa la opción que recibe al *linker*.

*   ¿Para qué sirve usar `&>`?

    El operador `&>` permite redirigir la salida de *standard output* y * error output* a un descriptor de archivo especificado.


    Los resultados de la compilación serán analizados en el archivo dummy.log. Para el análisis se buscarán palabras clave con el programa grep.

*   ¿Cuál es el efecto de los símbolos `., []` y `*` en los argumentos de `grep`?

    Se utilizan para formar expresiones regulares. Específicamente *Bracket Expressions*, *Anchoring* y *Repetition*.

*   ¿Qué hace la opción `–o` de `grep`?

    Indica que solo se debe de mostrar el texto que coincide con la búsqueda.

*   Explique el propósito de los archivos `crt1.o`, `crti.o` y `crtn.o`.

    Son los objetos del núcleo de c runtime, son necesarios para que las aplicaciones puedan ejecutarse.

*   En el comando `grep` con la opción `–B1`, ¿qué sucedería si no usamos esa opción?

    Imprime una línea de contexto antes de imprimir cada resultado, en caso que no se especifique leer el resultado sería muy complicado.

*   De acuerdo con estos resultados, ¿dónde se inicia la búsqueda de encabezados que  #incluimos en nuestros programas?

    Se inicia al hacer referencia a `linux-gnu`

*   ¿Qué  es  y  para  qué  sirve  un *soname*?  Explique  qué  son `libc.so.6` y `ld-linux.so.2` (se recomienda investigar el concepto de *shared library*).

    Es un campo de información en objetos compartidos.

*   ¿Qué es *underlinking* y por qué se considera mala práctica?

    Es cuando un archivo binario utiliza un símbolo que no es proporcionado por las bibliotecas.


**Instalación de Binutils**



*   ¿En qué consiste una PTY?

    Es un dispositivo de caracteres virtuales para comunicación de 2 vías (esclavo y maestro).

*   ¿Cómo funcionan las PTY?

    Se utilizan señales para comunicación entre el esclavo y el maestro.

*   ¿Cómo se relacionan con expect?

    expect se utiliza justamente para comunicación de programas, este espera un valor basado en un guión previamente especificado.

*   ¿Cómo se relacionan con el sistema de archivos devpts que montamos casi al inicio de la construcción del LFS?

    Es un sistema de archivos virtual en el que se especifican las representaciones de los esclavos de un maestro.


Se ejecutará la test suite usando la opción `-k` del comando make



*   ¿Qué hace la opción `–k`?

    Le indica a `make` que debe de continuar todo lo que pueda, incluso después de un error.


Luego de Binutils se instalará GMP y se ejecutará su test suite con el siguiente comando:

```
make check 2>&1 | tee gmp-check-log
```



*   ¿Qué diferencia hay entre usar `&>` y `2>&1`, y cuál es el efecto de esto último combinado con el comando `tee`?

    `2>&1` indica que el *standard error* debe de re dirigirse al *standard output*, combinado con `tee` redirige todo el *output* (*standard output* y *standard error*) al archivo.


Antes de la nueva instalación de GCC se instala el paquete Shadow para manejo seguro de contraseñas.:



*   ¿Qué son `/var/spool/mail` y `/var/mail`; y cuál es su relación con `Shadow`?

    Es la ubicación del buzón de entrada de Shadow. `/var/spool/mail` es una ubicación obsoleta, actualmente se utiliza `/var/mail`


**Instalación de GCC**



*   ¿Qué es y para qué sirve LTO (*Link Time Optimization*)?

    Es la capacidad de gcc en guardar la representación interna de los objetos, para que estos puedan ser optimizados en un solo módulo al momento de *link*.

*   ¿Para qué sirve Pkg-config?

    Se utiliza para obtener información de paquetes instalados, utiliza los archivos de *metadata* para obtener la información.


Luego de Pkg-config se  instalará Ncurses. Luego de instalarse este paquete se ejecutarán las siguientes instrucciones para asegurar que aplicaciones que funcionan sin *wide-character libraries*:

```bash
for lib in ncurses form panel menu ; do

	rm -vf                	/usr/lib/lib${lib}.so

	echo "INPUT(-l${lib}w)" > /usr/lib/lib${lib}.so

	ln -sfv ${lib}w.pc    	/usr/lib/pkgconfig/${lib}.pc

done
```



*   Lea  el  contenido  de  uno  de  los  archivos  creados  con  la  instrucción  anterior. ¿Qué hace la instrucción allí contenida?

    Cambia el valor de las variables en el makefile, y las banderas para que los programas puedan encontrar *wide-character libraries*.

*   ¿Qué es un *linker script* y para qué sirve?

    Se utiliza para indicar al *linker* los destinos y tipos de archivos que deben de ser generados.

*   ¿Cuál es la diferencia entre *Curses* y *Ncurses*?

    *Ncurses* emula el comportamiento de *curses*, pero provee extensiones que no pueden ser proveídas a *curses* (debido a que requieren acceso interno a la librería).


Después de Ncurses se instala el paquete Attr:



*   ¿Para qué sirve este paquete? Incluya una breve explicación de qué son los *extended attributes* en un sistema de archivos.

    Permite el uso de *extended attributes*, implementa la habilidad de agregar pares de valores `nombre:valor` a objetos en sistema de archivos *XFS*. Estos pares de valores permiten agregar *metadata* a los *inodes* en el sistema de archivos.


La  instalación  de  Attr  es  seguida  de  la  instalación  de  Acl  para  manejo  de  listas  de  control  de  acceso,  y luego por la instalación de *Libcap* para manejo de capabilities.



*   ¿Qué son las listas de control de acceso (ACL’s)?

    Una lista de control de acceso provee un sistema de permisos adicional y más flexible a un sistema de archivos. Permite aplicar permisos más específicos sin la necesidad de cambiar el dueño del archivo o los permisos originales.

*   Explique la relación entre listas de control de acceso y capabilities.

    capabilities se refiere al principio de diseño para compartir permisos entre usuarios, para lo que es necesario el control de acceso.


Se instalarán varios paquetes luego de *Libcap*, incluyendo *Flex* y *Libtool*.



*   ¿Cuál es la diferencia entre *Lex* y *Flex*?

    *Flex* se utiliza para generar programas que reconocen patrones en texto, *lex* fue el predecesor de *flex*. *flex* provee una opción para ser ejecutado emulando *lex*.

*   ¿Cuál es el propósito/beneficio que provee el *GNU generic library support script*?

    Provee una abstracción a la complejidad de utilizar librerías compartidas, provee una interfaz que es portable, y consistente. Elimina la necesidad de hacer implementaciones específicas para cada plataform cuando se requiere utilizar librerías compartidas.


**Construcción de LFS (parte II)**

Durante la instalación del paquete `Inetutils` En la sección 6.39, será necesario mover algunos programas a los directorios `/bin`y `/sbin`.



*   ¿Cuál es el propósito del directorio `/sbin`?

    Contienen *ejecutables* (al igual que `/bin`) que son solo accesibles por el usuario *root*


**Instalación de Perl**



*   ¿Cuál es el propósito del archivo `/etc/hosts`?

    Asocia la dirección ip a un *hostname*, se utiliza como una tabla estatica para realizar consultas de los *hostname*.

*   ¿Qué hacen las opciones `–des` y qué diferencia habría si no se usan?

    El script de configuración por defecto es interactivo, la opción `-des` utiliza los valores por defecto para la configuración. En caso de no especificarse este requeriría que se configure durante la ejecución del script.

*   El libro indica que la opción `–Dpager` de arriba sirve para usar `less` en lugar de more. ¿Qué es el *pager* de Perl?

    Es un programa para ver el contenido de un archivo, pero no para escribirlo.


**Instalación de Autoconf y Automake**



*   Describa el propósito de, y la relación entre, Autoconf y Automake

    Autoconf es un paquete de macros para generar scripts que configuran archivos de fuente automáticamente. Automake es utilizado para generar automaticamente el archivo `Makefile.in`; Automake utiliza Autoconf para la configuracion de los archivos que genera.


Luego de Automake se instalarán varias otras herramientas, incluyendo Gawk, Groffy Texinfo



*   Gawk es la implementación GNU de Awk. Describa Awk.

    Es un lenguaje de programación de propósito específico, es utilizado para procesamiento de texto, y coincidencia de patrones.

*   ¿Cuál es la importancia de Groff (y sus predecesores) en el contexto de  los sistemas Unix y sus derivados?

    Se utiliza para aplicar formatos sobre *plain text*, es de gran importancia para los sistemas *unix* debido a su asociación con los manuales de unix (*man pages*).

*   Explique brevemente la diferencia entre *man pages* e *info pages*.

    *man pages* es el estándar original de unix para los manuales del sistema, *info pages* es el estándar diseñador por GNU para la distribución de documentación. La diferencia entre ambos es principalmente en los formatos que se utilizan para la documentación, y la informacion que incluyen. regularmente la información que provee *info pages* es mas extensa que *man pages*.


Todos  hemos  usado  (o  usaremos)  el  editor  de  texto  Vi.  La  instalación  de  este  programa,  mediante  el paquete Vim, requiere especificar la ubicación por defecto del archivo `vimrc`.



*   ¿Qué es `vimrc`?

    Es el archivo de inicialización de vim, este puede contener diferentes configuraciones o comandos  a ejecutar cuando vim se inicializa.

*   ¿Qué ventajas presenta Vim sobre Vi?

    Entre alguna de las mejoras sobre vi se encuentran:

*   *undo* ilimitados
*   portabilidad, vi solo está presente en unix, vim puede utilizarse en windows, macos entre otros.
*   *syntax highlighting*
*   Soporte para una interfaz gráfica.

	

Después  de  Vim  toca  Procps-ng.  Para  este  paquete  se  hacen  unas  modificaciones  que  eviten  fallos  de pruebas que requieren el uso de dispositivos tty.



*   ¿Qué es un dispositivo `tty`?

    Un dispositivo de entrada y salida de texto para interactuar con el sistema.


Un  importante  paquete  a  instalarse  es  Util-linux. La  instalación  comienza  por  crear  algunos  directorios para almacenar adjtime y hwclock.



*   ¿Qué hacen el archivo de configuración adjtime y el programa hwclock?

    Evita que se muestran advertencias al instalar paquetes que no están disponibles en LFS; `hwclock` es el archivo de configuración del hardware del sistema.


Al instalarse el paquete Util-linux se hará sin soporte para `systemd`, el programa que hemos hablado en clase que reemplaza al tradicional `init` en algunos sistemas Linux modernos.



*   ¿Qué es la filosofía UNIX?

    Se originó durante el diseño del sistema operativo unix, se basa en estos principios:

*   Realizar cada programa que realice una cosa bien; para objetivos nuevos escribir un programa nuevo que realice la tarea bien, en lugar de agregar complejidad a otro programa.
*   Esperar que la salida de un programa se convierta en la entrada de otro (aún desconocido).
*   Diseñar y construir programas para que sean probados lo antes posible.

	Resumido por uno de sus originales ‘autores’ Doug McIlroy: Escribir programas que hagan una cosas y la hagan bien; diseñar programas para que funcionen con otros; escribir programas que procesen texto (es interfaz universal).



*   ¿Por qué hay quienes consideran que `systemd` va en contra de esta filosofía?

    El objetivo de `systemd` es unificar configuración y comportamiento de distintas distribuciones. El ser un programa grande que realiza varias tareas muchas personas consideran que va en contra de la filosofía unix en crear varios programas pequeños que funcionen entre sí.  

*   El libro indica que para ejecutar los tests como rootes necesario que el sistema en el que se está trabajando tenga habilitada como módulo la opción `CONFIG_SCSI_DEBUG`. ¿Qué son `SCSI` y los `scsi_debug` *devices*?

    `scsi_debug` es un emulador de controlador de disco que simula variables de disco SCSI

*   Liste al menos tres programas de uso común que son instalados por este paquete. Puede incluir los que hemos usado a lo largo del proyecto y en laboratorios.

    python, bash, nano


**Instalación de E2fsprogs**



*   ¿Para qué sirve el archivo dir ubicado en /usr/share/info?

    Guarda la información del usuario que puede ser compartida con el grupo de usuarios del sistema.

*   ¿Cuál es la diferencia entre un sistema de archivos común y un sistema de archivos que lleva un diario (journaling file system)?

    Al llevar una lista de los cambios en el sistema de archivo un sistema con dirario es menos probable a corromperse.


Luego  de  E2fsprogs  se  instalarán  los  paquetes  Sysklogd  y  Sysvinit



*   Explique el contenido y propósito de este archivo de configuración.

    Almacena la configuración de `syslogd` que recibe los mensajes del sistema.

*   Explique los run levels y bootscripts de SystemV.¿Qué es SystemV?

    `SystemV` fue una de las primeras implementaciones de unix desarrolada por bell labs (At&t)


**Instalación de Eudev**



*   Explique brevemente para qué sirve y por qué es importante este paquete. ¿Cuál es la relación entre udev y systemd?

    udev gestiona los nodos de dispositivo en `/dev`, una de las partes principales de `systemd` es el administrador de sistemas de archivos para lo que se vuelve necesario eudev.

*   Luego  de  compilar  el  paquete se  ejecutan  unas instrucciones que instalan  reglas  y  archivos auxiliares necesarios. ¿Qué son “reglas” y cuál es el propósito de las que se instalan?

    Son directivas que determinan los protocolos que se utilizan para el manejo de archivos en paquetes instalados.

*   La  última  instrucción  crea  la  base  de  datos  inicial  de  dispositivos  de hardware en  el  archivo /etc/udev/hwdb.bin. ¿Qué información almacena este archivo?

    Se utiliza para almacenar valores clave de los dispositivos. 


**Últimos pasos**

Completando el LFS se indica que es necesario identificar el nombre que Udev asigna a los dispositivos de red, en el archivo/etc/udev/rules.d/70-persistent-net.rules.Más adelante se requiere la creación de archivos de configuración para la interfaz de red, en el directorio /etc/sysconfig.



*   ¿Cuál es el propósito de este archivo de configuración en el directorio `/etc/sysconfig`?

    Especifica las configuraciones del sistema, como puede ser la configuración del reloj.


Configurar  el  sistema  implica  crear  el  archivo inputrc,  que  sirve  para  configuración  del  programa Readline Instalado previamente.



*   ¿Cuál es la importancia del paquete Readline?

    Este paquete permite el uso de aplicaciones que modifican los comandos a medida que el usuario las escribe.


**Referencias:**

[https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php](https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php)

[https://linux.die.net/man/1/test](https://linux.die.net/man/1/test)

[https://www.tldp.org/LDP/abs/html/special-chars.html](https://www.tldp.org/LDP/abs/html/special-chars.html)

[https://www.gnu.org/software/tar/manual/tar.html#SEC8](https://www.gnu.org/software/tar/manual/tar.html#SEC8)

[https://www.gnu.org/software/tar/manual/tar.html#SEC134](https://www.gnu.org/software/tar/manual/tar.html#SEC134)

[https://linux.die.net/man/1/tar](https://linux.die.net/man/1/tar)

[https://blog.usejournal.com/what-is-the-difference-between-a-hard-link-and-a-symbolic-link-8c0493041b62](https://blog.usejournal.com/what-is-the-difference-between-a-hard-link-and-a-symbolic-link-8c0493041b62)

[https://linux.die.net/man/1/ln](https://linux.die.net/man/1/ln)

[https://friendly-101.readthedocs.io/en/latest/bashprofile.html](https://friendly-101.readthedocs.io/en/latest/bashprofile.html)

[https://www2.cs.duke.edu/csl/docs/unix_course/intro-60.html](https://www2.cs.duke.edu/csl/docs/unix_course/intro-60.html)

[https://medium.com/@codenameyau/step-by-step-breakdown-of-dev-null-a0f516f53158](https://medium.com/@codenameyau/step-by-step-breakdown-of-dev-null-a0f516f53158)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter04/addinguser.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter04/addinguser.html)

[https://www.softwaretestinghelp.com/unix-permissions/](https://www.softwaretestinghelp.com/unix-permissions/)

[https://linux.die.net/man/1/bash](https://linux.die.net/man/1/bash)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter04/settingenvironment.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter04/settingenvironment.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter04/settingenvironment.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter04/settingenvironment.html)

[http://man7.org/linux/man-pages/man2/umask.2.html](http://man7.org/linux/man-pages/man2/umask.2.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter04/settingenvironment.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter04/settingenvironment.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/binutils-pass1.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/binutils-pass1.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/toolchaintechnotes.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/toolchaintechnotes.html)

[https://wiki.osdev.org/Target_Triplet](https://wiki.osdev.org/Target_Triplet)

[https://news.ycombinator.com/item?id=13239791](https://news.ycombinator.com/item?id=13239791)

[https://vjordan.info/log/fpga/cpu-vendor-os.html](https://vjordan.info/log/fpga/cpu-vendor-os.html)

[http://www.tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_04.html](http://www.tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_04.html)

[https://linux.die.net/man/1/sed](https://linux.die.net/man/1/sed)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/gcc-pass1.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/gcc-pass1.html)

[https://linux.die.net/man/1/touch](https://linux.die.net/man/1/touch)

[https://linux.die.net/man/1/cp](https://linux.die.net/man/1/cp)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/gcc-pass1.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/gcc-pass1.html)

[https://gcc.gnu.org/onlinedocs/gccint/Driver.html](https://gcc.gnu.org/onlinedocs/gccint/Driver.html)

[https://gcc.gnu.org/install/configure.html](https://gcc.gnu.org/install/configure.html)

[https://kb.iu.edu/d/akqn](https://kb.iu.edu/d/akqn)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/toolchaintechnotes.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/toolchaintechnotes.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/glibc.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/glibc.html)

[https://linux-audit.com/elf-binaries-on-linux-understanding-and-analysis/#what-is-an-elf-file](https://linux-audit.com/elf-binaries-on-linux-understanding-and-analysis/#what-is-an-elf-file)

[https://linux.die.net/man/1/readelf](https://linux.die.net/man/1/readelf)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/binutils-pass2.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/binutils-pass2.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/gcc-libstdc++.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/gcc-libstdc++.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/binutils.html#contents-binutils](http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/binutils.html#contents-binutils)

[https://linux.die.net/man/3/dirname](https://linux.die.net/man/3/dirname)

[https://www.tldp.org/LDP/abs/html/special-chars.html](https://www.tldp.org/LDP/abs/html/special-chars.html)

[https://www.cplusplus.com/reference/climits/](https://www.cplusplus.com/reference/climits/)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/gcc-pass2.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/gcc-pass2.html)

[http://www.linuxfromscratch.org/blfs/view/9.0/general/tcl.html](http://www.linuxfromscratch.org/blfs/view/9.0/general/tcl.html)

[https://www.tcl-lang.org/about/audience.html](https://www.tcl-lang.org/about/audience.html)

[http://www.linuxfromscratch.org/lfs/view/development/chapter05/expect.html](http://www.linuxfromscratch.org/lfs/view/development/chapter05/expect.html)

[https://invisible-island.net/ncurses/announce.html#h2-overview](https://invisible-island.net/ncurses/announce.html#h2-overview)

[https://linux.die.net/man/3/malloc](https://linux.die.net/man/3/malloc)

[https://www.gnu.org/software/m4/m4.html](https://www.gnu.org/software/m4/m4.html)

[https://www.gnu.org/software/coreutils/manual/html_node/hostname-invocation.html#hostnme-invocation](https://www.gnu.org/software/coreutils/manual/html_node/hostname-invocation.html#hostnme-invocation)

[https://www.gnu.org/software/diffutils/manual/diffutils.html#Invoking-patch](https://www.gnu.org/software/diffutils/manual/diffutils.html#Invoking-patch)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/gettext.html#contents-gettex](http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/gettext.html#contents-gettex)

[https://www.gnu.org/software/guile/](https://www.gnu.org/software/guile/)

[https://www.gnu.org/software/make/manual/html_node/Pattern-Intro.html#Pattern-Intro](https://www.gnu.org/software/make/manual/html_node/Pattern-Intro.html#Pattern-Intro)

[https://www.tutorialspoint.com/gnu_debugger/gdb_debugging_symbols.html](https://www.tutorialspoint.com/gnu_debugger/gdb_debugging_symbols.html)

[https://linux.die.net/man/1/strip](https://linux.die.net/man/1/strip)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/stripping.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter05/stripping.html)

[https://linux.die.net/sag/filesystems.html](https://linux.die.net/sag/filesystems.html)

[https://linux.die.net/man/8/mount](https://linux.die.net/man/8/mount)

[https://linux.die.net/sag/dev-fs.html](https://linux.die.net/sag/dev-fs.html)

[http://man7.org/linux/man-pages/man5/sysfs.5.html](http://man7.org/linux/man-pages/man5/sysfs.5.html)

[https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03s15.html](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03s15.html)

[https://linux.die.net/man/1/mknod](https://linux.die.net/man/1/mknod)

[https://linux.die.net/man/2/mknod](https://linux.die.net/man/2/mknod)

[http://www.linux-tutorial.info/modules.php?name=MContent&pageid=94](http://www.linux-tutorial.info/modules.php?name=MContent&pageid=94)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/chroot.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/chroot.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/createfiles.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/createfiles.html)

[https://linux.die.net/man/1/install](https://linux.die.net/man/1/install)

[https://linux.die.net/sag/etc-fs.html](https://linux.die.net/sag/etc-fs.html)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/createfiles.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/createfiles.html)

[https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)

[https://linux.die.net/sag/etc-fs.html](https://linux.die.net/sag/etc-fs.html)

[https://linux.die.net/man/5/passwd](https://linux.die.net/man/5/passwd)

[https://linux.die.net/man/5/group](https://linux.die.net/man/5/group)

[https://linux.die.net/man/1/patch](https://linux.die.net/man/1/patch)

[https://linux.die.net/man/1/touch](https://linux.die.net/man/1/touch)

[https://linux.die.net/man/8/nscd](https://linux.die.net/man/8/nscd)

[https://linux.die.net/man/5/nscd.conf](https://linux.die.net/man/5/nscd.conf)

[https://linux.die.net/man/8/zic](https://linux.die.net/man/8/zic)

[https://linux.die.net/man/8/tzselect](https://linux.die.net/man/8/tzselect)

[https://linux.die.net/man/1/gc](https://linux.die.net/man/1/gc)

[https://linux.die.net/man/1/bash](https://linux.die.net/man/1/bash)

[https://linux.die.net/man/1/grep](https://linux.die.net/man/1/grep)

[https://linux.die.net/man/1/grep](https://linux.die.net/man/1/grep)

[https://docs.oracle.com/cd/E88353_01/html/E37853/crti.o-7.html](https://docs.oracle.com/cd/E88353_01/html/E37853/crti.o-7.html)

[https://linux.die.net/man/1/grep](https://linux.die.net/man/1/grep)

[https://linux.die.net/man/1/make](https://linux.die.net/man/1/make)

[https://linux.die.net/man/1/tee](https://linux.die.net/man/1/tee)

[https://gcc.gnu.org/wiki/LinkTimeOptimization](https://gcc.gnu.org/wiki/LinkTimeOptimization)

[https://linux.die.net/man/1/pkg-config](https://linux.die.net/man/1/pkg-config)

[https://linux.die.net/man/3/ncurses](https://linux.die.net/man/3/ncurses)

[https://linux.die.net/man/1/attr](https://linux.die.net/man/1/attr)

[https://xfs.org/docs/xfsdocs-xml-dev/XFS_Filesystem_Structure/tmp/en-US/html/ExtendedAttributes.html](https://xfs.org/docs/xfsdocs-xml-dev/XFS_Filesystem_Structure/tmp/en-US/html/ExtendedAttributes.html)

[https://www.redhat.com/sysadmin/linux-access-control-lists](https://www.redhat.com/sysadmin/linux-access-control-lists)

[http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/flex.html](http://www.linuxfromscratch.org/lfs/view/9.0/chapter06/flex.html)

[https://www.gnu.org/software/libtool/manual/html_node/Introduction.html#Introduction](https://www.gnu.org/software/libtool/manual/html_node/Introduction.html#Introduction)

[http://www.linfo.org/sbin.html](http://www.linfo.org/sbin.html)

[https://linux.die.net/man/5/hosts](https://linux.die.net/man/5/hosts)

[https://metacpan.org/pod/distribution/perl/INSTALL](https://metacpan.org/pod/distribution/perl/INSTALL)

[https://www.gnu.org/software/autoconf/autoconf.html](https://www.gnu.org/software/autoconf/autoconf.html)

[https://www.gnu.org/software/automake/](https://www.gnu.org/software/automake/)

[https://www.gnu.org/software/gawk/](https://www.gnu.org/software/gawk/)

[https://www.gnu.org/software/groff/](https://www.gnu.org/software/groff/)

[https://www.gnu.org/prep/standards/standards.html#Man-Pages](https://www.gnu.org/prep/standards/standards.html#Man-Pages)

[https://www.gnu.org/software/texinfo/](https://www.gnu.org/software/texinfo/)

[https://linux.die.net/man/1/vi](https://linux.die.net/man/1/vi)

[https://www.vim.org/viusers.php](https://www.vim.org/viusers.php)

[https://homepage.cs.uri.edu/~thenry/resources/unix_art/ch01s06.html](https://homepage.cs.uri.edu/~thenry/resources/unix_art/ch01s06.html)

[https://linux.die.net/man/1/bash](https://linux.die.net/man/1/bash)