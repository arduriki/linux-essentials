# Index

- [El SO Linux](#el-so-linux)
  - [Comandos de componentes del sistema](#comandos-de-componentes-del-sistema)
    - [Directorio /etc -> configuración -> .conf](#directorio-etc---configuración---conf)
    - [Directorio /home](#directorio-home)
    - [Directorio /boot](#directorio-boot)
    - [Procesos del sistema](#procesos-del-sistema)
    - [Arranque de Linux](#arranque-de-linux)
    - [Registro del sistema](#registro-del-sistema)

- [Comandos básicos](#comandos-básicos)
  - [Echo -> Mostrar texto](#echo---mostrar-texto)
  - [Type](#type)
  - [Which](#which)
  - [Uname](#uname)
  - [Set](#set)
  - [Export](#export)
  - [Unset](#unset)
  - [Env](#env)
  - [Obtener ayuda](#obtener-ayuda)
  - [Moverse por los ficheros](#moverse-por-los-ficheros)
  - [Opciones listado](#opciones-listado)
  - [Comandos](#comandos)
  - [Crear y borrar ficheros y directorios](#crear-y-borrar-ficheros-y-directorios)
  - [Copiar -> cp](#copiar---cp)
  - [Comodín](#comodín)
    - [* -> Carácter opcional](#---carácter-opcional)
    - [? -> Carácter obligatorio](#---carácter-obligatorio)
    - [Clase](#clase)

- [Terminal power](#terminal-power)
  - [Compactar y comprimir](#compactar-y-comprimir)
  - [Mostrar texto](#mostrar-texto)
  - [Invertir y contar](#invertir-y-contar)
  - [Cortar](#cortar)
  - [Redirección](#redirección)
  - [Tuberías](#tuberías)
  - [Filtrar texto grep](#filtrar-texto-grep)
  - [Expresiones regulares = regex](#expresiones-regulares--regex)
  - [Ordenar y seleccionar líneas únicas](#ordenar-y-seleccionar-líneas-únicas)

- [Configuración por terminal](#configuración-por-terminal)
  - [Obsoleto](#obsoleto)
  - [Actual](#actual)
    - [Ping + Traceroute (MTR)](#ping--traceroute-mtr)
  - [Comando ip](#comando-ip)
    - [Comando ss (Socket Statistics)](#comando-ss-socket-statistics)
  - [Configuración de red](#configuración-de-red)
  - [Configuración persistente](#configuración-persistente)
    - [Modelo Debian](#modelo-debian)
  - [Configuración DNS](#configuración-dns)
    - [Resolución de DNS](#resolución-de-dns)

- [Seguridad y sistema de permisos de archivos](#seguridad-y-sistema-de-permisos-de-archivos)
  - [Usuarios y grupos](#usuarios-y-grupos)
  - [Cambio de propietario de usuario o grupo](#cambio-de-propietario-de-usuario-o-grupo)
  - [Permisos en ficheros](#permisos-en-ficheros)
  - [Permisos en directorios](#permisos-en-directorios)
  - [Cambiar permisos chmod](#cambiar-permisos-chmod)
    - [Chmod con notación binario o decimal](#chmod-con-notación-binario-o-decimal)
  - [Permisos especiales](#permisos-especiales)
  - [Seguridad en usuarios](#seguridad-en-usuarios)
    - [Logins](#logins)
  - [Administración de usuarios](#administración-de-usuarios)
    - [Ejemplo de creación de usuarios](#ejemplo-de-creación-de-usuarios)
    - [Contraseñas](#contraseñas)
      - [Ejemplo de contraseñas](#ejemplo-de-contraseñas)
  - [Administración de grupos y usuarios](#administración-de-grupos-y-usuarios)
  - [Enlaces en el sistema de ficheros de Linux](#enlaces-en-el-sistema-de-ficheros-de-linux)

---

## El SO Linux

### Comandos de componentes del sistema

```shell
# Números de interrupción del hardware
cat /proc/interrupts
# Canales activados
cat /proc/dma
# Dispositivos de entrada y salida
cat /proc/ioports

# Relacionado con el hardware:
ls -l /sys/
# Dispositivos están nombrados por clase.
ls -l /sys/class

# Dispositivos de red
ls -l /net
# Interfaces de red
ls -l /sys/class/net/  # Se puede entrar en los directorios que nos marcan
# Dispositivos
ls -l /sys/devices

# Ver las interfaces de red y sus detalles
ip -c a
# Buscar su directorio
find /sys/ -type d -name "nombre_interfaz"

# Otros dispositivos como la CPU, el disco duro, etc.
cd /dev/
```

### Directorio /etc -> configuración -> .conf

```shell
# Ver los usuarios del sistema
less /etc/passwd  # se añaden filas al final por cada nuevo usuario
# también se muestran usuarios para utilizar ciertas partes del sistema
# Ver el identificador del usuario
id {usuario}

# Ver grupos
less /etc/group

# Ver contraseñas encriptadas de los diferentes usuarios
less /etc/shadow

# Nombre de la máquina
cat /etc/hostname

# Ver los hosts que tiene registrados el systema.
# Se puede editar para que una dirección IP se traduzca por un nombre:
# 127.0.0.1 -> localhost
less /etc/hosts
```

### Directorio /home

```shell
# Comandos ejecutados, hasta 500 ordenes
less .bash_history

# Configuración del entorno
less .bashrc
```

### Directorio /boot

```shell
# Ver ficheros al arrancar
cd /boot/

# Configuración de gestión de arranque.
cd /boot/grub/

# Información sobre los procesos
cd /proc/

# Generar información vacía y ver lo generado
cat /dev/zero > /root/fichero-nulo
ls -lh /root/fichero-nulo
rm /root/fichero-nulo
```

### Procesos del sistema

``` shell
# Ver todos los procesos del sistema
sudo ps -e

# Ver el núm de comando, la terminal, el tiempo que lleva ejecutado, comando que ha provocado el proceso:
ps

# El usuario que ha ejecutado, PPID el proceso padre, C es el % de CPU utilizado, STIME momento en el que se creó el proceso.
ps -f

# Obtener ayuda
ps --help

# Comando con el path completo
ps aux
```

| Procesos | Descripción                                                            |
| -------- | ---------------------------------------------------------------------- |
| `ps`     | Muestra información sobre los procesos en ejecución                    |
| `pstree` | Muestra los procesos en una jerarquía                                  |
| `top`    | Información sobre procesos en tiempo real - se ordena por carga de CPU |
| `htop`   | Lo mismo que `top` pero con asteroides.                                |
| `free`   | Uso de la memoria                                                      |
| `uptime` | Tiempo encendido y carga del sistema                                   |
| `pgrep`  | Muestra sólo los procesos que cumplen un criterio -> `pgrep -l a`      |
Poner a prueba la máquina por 100 segundos:
`stress -c 2 -t 100 -i 1`

### Arranque de Linux

```bash
# Ver los mensajes de arranque de Linux:
dmesg
```

| Opciones | Descripción                                 |
| -------- | ------------------------------------------- |
| `-T`     | Muestra las marcas de tiempo más claramente |
| `-k`     | Sólo mensajes del kernel                    |
| `-l`     | filtra por niveles de aviso                 |

Comandos comunes

```shell
dmesg | less
dmesg -H  # En colores y paginado
dmesg -lerr
dmesg -lwarn

# dentro del dmesg, se puede ir a buscar cuándo se realiza el arranca por RAM
/initramfs  # Init RAM File System
```

### Registro del sistema

Registros guardados en `/var/log/`.
Fichero de configuración en `/etc/rsyslog.conf` o en ficheros dentro de `/etc/rsyslog.d/`.
Selector de mensajes consta de dos partes: `facility.priority`.

Ejemplos:

| Selector                    | Descripción                                          |
| --------------------------- | ---------------------------------------------------- |
| `*.*`                       | Todos los mensajes                                   |
| `*.info`                    | Todos los mensajes de info                           |
| `kern.*`                    | Todos los mensajes del kernel                        |
| `mail.err`                  | Todos los mensajes de error del correo               |
| `cron,lpr.warn`             | Los warning de cron y de lpr (impresora)             |
| `cron.err;cron.!alert`      | Los errores de cron, pero NO las alertas             |
| `mail.=err`                 | Solo los errores de mail                             |
| `*.info;mail.none;lpr.none` | Todos los mensajes de info excepto los de mail y lpr |

Práctica:

```shell
# Ver el estado de syslog
systemctl status rsyslog

# Ver el archivo de configuración
less /etc/rsyslog.conf
```

Dentro de la sección **Rules** se pueden ver hacia donde se redirigen los mensajes.

```shell
# Directorio donde hay los logs
tail /var/log/syslog

# Para ver en vivo el log y los mensajes entrantes
tail -f /var/log/syslog
```

**journalctl** -> Consultar los log del sistema

| commandos     | descripción                                                         |
| ------------- | ------------------------------------------------------------------- |
| `-S -U`       | permite especificar desde (since) y/o hasta cuando (until)          |
| `-u`          | mensaje de una unidad en concreto                                   |
| `-k`          | mensajes del kernel                                                 |
| `-p`          | por tipo (emerg, alert, crit, err, warning, notice, info, debug)    |
| `PARAM=VALUE` | Parámetros como `_PID, _UID, _COMM` -> `man systemd.journal-fields` |

Ejemplos

```shell

# Acceder
journalctl

# Determinar un rango de fechas
journalctl -S 2024-09-01 -U 2024-10-21
# Hoy
journalctl -S today
# Hace unos días
journalctl -S '20 day ago'
# Hace unas horas y minutos
journalctl -S -6h25min

# Ver una unidad en concreto
journalctl -u networking.service
journalctl -u cron.service

# Ver el kernel = el hardware
journalctl -k

# Ver errores
journalctl -p err
journalctl -p warning
journalctl -p info

# Para ver los tipos de entradas que podemos observar
man systemd.journal-fields
# Ver las entradas por comandos
journalctl _COMM={comando}
# Ver por PID
journalctl _PID={num_pid}
# Ver por usuario
id {usuario}
journalctl UID={id_usuario}
```

[Vuelta al índice](#index)

---

## Comandos básicos

### Echo -> Mostrar texto

```bash
# Muestra el texto que recibe literalmente
echo "Esto es un ejemplo"
echo $USER  # cual es el usuario actual
echo $PATH  # archivos ejecutables

# Interpreta los caracteres especiales: saltos de línea, tabuladores, etc.
echo -e "Esto es \n un ejemplo"

# Comillas dobles:
echo "rutas ==> $PATH"
# Resultado:
rutas ==> /usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games

# Comillas simples: no hace nada
echo 'rutas ==> $PATH'
# Resultado:
rutas ==> $PATH
# Más comillas simples: ignora la variable.
echo -e 'El usuario: \n $USER'
El usuario: 
 $USER
```

### Type

Va seguido del nombre del comando para saber a qué categoría pertenece dicho comando.

```bash
type cd
```

### Which

Muestra la ruta absoluta del programa/comando que le indiquemos.

```bash
which ls
```

### Uname

Muestra información sobre el S.O.

```bash
# Mostrar todo:
uname -a

# Mostrar versión S.O.
cat /etc/issue
# + info
cat /etc/os-release
```

### Set

Muestra o modifica la configuración de nuestro entorno.

```bash
# Visualiza variables del sistema
set

# Lista de las opciones y sus estados.
set -o

# Activar opción
set -o <opción>

# Desactivar opción
set +o <opción>
```

### Export

Crea o modifica variables de entorno.

```bash
export NOMBRE=Antonio
```

### Unset

Elimina una variable de entorno.

```bash
unset NOMBRE
```

### Env

Variables de entorno:

```bash
# Muestra las variables de entorno:
env

# Modificar el valor de una variable de entorno:
env PATH=/new/path /bin/bash

# Salir del entorno:
exit

# Ver los directorios del PATH
echo $PATH

# Crear un archivo ejecutable
nano comando.sh

# Después de editar el archivo, hacerlo ejecutable:
chmod +x comando.sh

# Si el comando está en el directorio actual, para ejecutarlo:
./comando  # O la ruta absoluta

# Ver el historial de ordenes.
history

# En la carpeta home, ver los comandos ejecutados:
less .bash_history
```

### Obtener ayuda

```shell
# Manual
man <comando/fichero>

# Ir a la sección
man número <comando/fichero>

# Descripción corta del comando
man -f comando

# Buscar comandos que tengan que ver con lo que queremos realizar.
man -k palabra_clave  # La palabra clave debe aparecer en la descripción del comando.

# Ayuda más extensa
info <comando/fichero>

# Actualizar el sistema de ficheros en la terminal
updatedb

# Buscar dónde se encuentra algo en el sistema de ficheros
locate palabra_clave

# Ayuda rápida
comando -h  # también --help

# Muestra descripciones de una línea de las páginas de manual
whatis comando

# Busca nombres y descripciones de páginas de manual
apropos palabra_clave

# Localiza el binario, la fuente y el manual de un comando
whereis comando
```

Estructura de la página manual:

| Sección     | Descripción                                    |
| ----------- | ---------------------------------------------- |
| NAME        | Nombre del comando y breve descripción         |
| SYNOPSIS    | Descripción de la sintaxis del comando         |
| DESCRIPTION | Descripción de los efectos del comando         |
| OPTIONS     | Opciones disponibles                           |
| ARGUMENTS   | Argumentos disponibles                         |
| FILES       | Archivos auxiliares                            |
| EXAMPLES    | Una muestra de la línea de comando             |
| SEE ALSO    | Referencias cruzadas a los temas relacionados. |
| DIAGNOSTICS | Mensajes de advertencia y error                |
| COPYRIGHT   | Autor/es del comando                           |
| BUGS        | Cualquier limitación conocide del comando      |

Consultar categorías de las páginas man:

| Categoría | Descripción                                             |
| --------- | ------------------------------------------------------- |
| 1         | Comandos Generales                                      |
| 2         | Llamadas al sistema                                     |
| 3         | Llamadas a librerías del sistema                        |
| 4         | Ficheros especiales (normalmente dispositivos, en /dev) |
| 5         | Formatos de ficheros y convenciones                     |
| 6         | Juegos                                                  |
| 7         | Miscelánea                                              |
| 8         | Comandos de administración del sistema y Daemons        |
| 9         | Rutinas del kernel                                      |

### Moverse por los ficheros

```shell
# Ruta del directorio actual
pwd

# Listar directorios en la ruta actual
ls

# Directorio home
cd

# Change directory
cd <directorio>/

# Subir un nivel
cd ..

# Ir a la raíz del sistema
cd /

# Ir al directorio que he estado anteriormente
cd -
```

### Opciones listado

``` shell
# visto por lista (orden alfabetico)
ls -l

# Ver el peso del archivo en bytes:
ls -lh

# Ordenar por tamaño
ls -lS

# Ordenar por la fecha
ls -lt

# Ordenar de forma inversa
ls -lr

# Listado recursivo
ls -lR

# Muestra TODOS los elementos
ls -a
```

### Comandos

```shell
# Ver el tipo de comando:
type comando

# Conseguir privilegios
su
su -  # Ir a la home de root

# En comandos con permisos denegados:
sudo comando 
```

### Crear y borrar ficheros y directorios

```shell
# Crear directorio
mkdir ruta_existente/directorio_crear

# Crear varios directorios a la vez en la ruta
mkdir -p ruta_crear/directorio_crear/

# Crear un fichero vacío
touch nombre.extension
# alternativa
> nombre.extension

# Renombrar
mv elemento_cambiar nuevo_nombre
# Mover / Cortar a un nuevo directorio
mv elemento_cambiar nueva_ruta/
# Cambiar y renombrar
mv elemento_cambiar nueva_ruta/nuevo_nombre

# Borrar fichero
rm fichero_borrar

# Borrar directorio
rmdir directorio_vacío
# Borrar directorio con elementos
rmdir -r directorio_borrar

# Mostrar toda la jerarquía de ficheros en una carpeta
tree carpeta  # es posible que se tenga que instalar "tree"
```

### Copiar -> cp

```bash
# Se pueden usar rutas relativas o absolutas
cp origen destino/[nuevo_nombre]

# Copiar varios elementos a la vez
cp origen origen origen destino

# Copiar directorios completos
cp -r origen/ destino/

# Copiar TODOS los archivos de una extensión en un directorio
```

### Comodín

#### * -> Carácter opcional

```shell
# Seleccionar TODOS
*

# Seleccionar un tipo de extensión
*.extension

# Seleccionar que contenga un cierto caracter en el nombre
*u*

# También se aplica en la extensión
*u*.*i*

# Que exista un archivo
*.*
```

#### ? -> Carácter obligatorio

```shell
# Ejemplo para listar:
ls -l c?sa*  # ? -> cualquier caracter por fuerza
# * -> PUEDE que no haya un caracter
ls -l informe-200?.txt

# Se puede practicar con un "echo" para asegurarse a ver qué se selecciona
echo *.d*
```

#### Clase

```shell
# Letras y números.
[[:alnum:]]

# Letras mayúsculas o minúsculas.
[[:alpha:]]

# Espacios y tabulaciones.
[[:blank:]]

# Números (0123456789)
[[:digit:]]

# Letras minúsculas (a-z)
[[:lower:]]

# Letras mayúsculas (A-Z)
[[:upper:]]

# ejemplo de corchetes con los backups
ls -l backup_20??-??-?[24680]*  # Cualquier año 2000, cual mes, día número par
```

[Vuelta al índice](#index)

---

## Terminal power

### Compactar y comprimir

Útil para realizar copias de seguridad.

```shell
tar [opciones] destino.tar datos/ficheros

# Compactar
tar -cf fichero.tar /etc/ /var/
# Extraer
tar -xf fichero.tar

# Comprimir con gzip
tar -czf fichero.tar.gz /etc/ /var/
# Extraer
tar -xzf fichero.tar.gz

# Añadimos -v para ver el proceso y los archivos con los que se trabaja.
```

Opciones más importantes:

| Opción | Descripción                                             |
| ------ | ------------------------------------------------------- |
| `-c`   | compacta                                                |
| `-x`   | expande (extrae)                                        |
| `-f`   | escribe o lee de un fichero                             |
| `-z`   | comprime o descomprime con gunzip                       |
| `-j`   | comprime o descomprime con bzip2                        |
| `-P`   | utiliza rutas absolutas (relativas por defecto)         |
| `-p`   | preserva los permisos de los ficheros originales        |
| `-r`   | añade elementos a un fichero compactado (No comprimir!) |
| `-t`   | muestra información que contiene un fichero tar         |

```shell
# Ejemplo de copiar un fichero según una fecha de modificación
sudo tar -czf copia.tar.gz /var/log/ -N 2018-04-01

# Ejemplo de copia incremental
sudo tar -czf copia_dif.tar.gz /var/log/ -g registro_inc.log

# Consultar lo que contiene un fichero
tar -tf copia.tar | less

# Buscar algo en concreto
tar -tf copia.tar | grep Documentos

# Comprimir un archivo compactado con Gzip
gzip {archivo.tar}
```

Comandos para comprimir y descomprimir (gzip, bzip, xz):

```shell
# Comprimir y conservar el original
gzip -k fichero
bzim -k fichero

# Descomprimir
gunzip fichero.gz
bzip -d fichero.gz
```

### Mostrar texto

```shell
# Mostrar el contenido de un archivo de texto
cat {rutaFichero}

# Muestra el texto con números de línea
cat -n {rutaFichero}

# Si un archivo es muy grande, verlo por partes
more {rutaFichero}  # Una vez dentro, 'h' para ver las opciones
less {rutaFichero}  # Más eficiente

# Primeras 10 líneas
head {rutaFichero}
# Ver la primera línea
head -n1 {rutaFichero}
# Indicar el número de líneas
head 30 {rutaFichero}
# Ver diferentes ficheros, las 5 primeras líneas
head -n5 {rutaFichero}/*.log  # Ejemplo
# Solamente el contenido
head -n -q {rutaFichero}/*.log  # Ejemplo

# Ultimas 10 líneas
tail {rutaFichero}  # Mismas opciones que "head"
```

### Invertir y contar

Útil para archivos muy estructurados, como las BD

```bash
# Lineas, palabras y caracteres
wc {rutaFichero}
# Solo líneas:
wc -l {rutaFichero}
# Solo palabras:
wc -w {rutaFichero}
# Solo caracteres:
wc -m {rutaFichero}

# Invertir
cut -d"/" -f1 {rutaFichero}  # número indica los niveles hacia abajo
rev {rutaFichero}
```

### Cortar

```shell
# Corte "vertical" a partir de un caracter
cut fichero.txt  # inicio o fin de linea
cut -c7-25 {rutaFichero}  # a partir de un caracter hasta el indicado de cada linea
cut -c5,10 {rutaFichero}  # números independientes de cada linea

# Dividir por caracter delimitador
cut -d"{caracterDelimitador}" -f1 {rutaFichero}  # f1 escoge el primero de la linea
cut -d":" -f1 /etc/passwd  # Ejemplo1
cut -d":" -f1,4,7,8-10 /etc/passwd  # Ejemplo2
```

### Redirección

La salida de texto suele aparecer en pantalla. Se puede cambiar hacia otro lugar, por ejemplo un fichero.

```shell
# Se crea el archivo
echo "Hola Mundo" > saludo.txt
# Se añade al archivo creado
echo "¿Cómo estás?" >> saludo.txt

# Mensajes de error 2> o 2>>
cp Downloads ./copias 2> log.txt  # Ejemplo

# TODOS los mensajes &> o &>>
cat /etc/s* &> cat.todo  # Ejemplo
```

### Tuberías

Es una redirección hacia otro comando de Linux -> |

```shell
# Ejemplos
echo "HOLA MUNDO" | wc
grep ^a /etc/passwd | cut -d":" -f1
du -sh /* 2> /dev/null | sort -h | tail -n1  # Ver el directorio más grande
```

Para **desechar** alguna salida, redirigir a **/dev/null**. No aparecerá en ningún lugar:

```shell
grep -r max_size /etc/* &> /dev/null

# Combinar todo
wc /etc/a* 2> /dev/null | sort -n > count
```

### Filtrar texto: grep

Muestra sólo las lineas que cumplen con un patrón -> Con palabras o regex (expresiones regulares).

| Opción | Descripción                                                               |
| ------ | ------------------------------------------------------------------------- |
| `-v`   | Muestra las que NO coinciden con el patrón                                |
| `-l`   | Sólo indica el nombre del fichero donde ha encontrado alguna coincidencia |
| `-w`   | El patrón tiene que ser una palabra independiente                         |
| `-n`   | Indica el número de linea                                                 |
| `-i`   | No distingue entre mayúscula y minúscula                                  |
| `-c`   | Muestra la cantidad de lineas que cumplen con el patrón.                  |
| `-r`   | Busca en los ficheros de forma recursiva                                  |

Ejemplos:

```shell

# Buscar "root", sin discriminar en la posición.
grep root /etc/passwd
# Buscar "al", se mostrará en color
grep al /etc/passwd

# NO hay patrón "al"
grep al -v /etc/passwd

# Muestre el fichero donde hay coincidencia
grep root -l /var/log/*.log

# Para buscar cierto grupo de usuarios, solamente buscar 100
grep 100 -w /etc/passwd

# Indicar la linea en el archivo
grep root -n /etc/passwd

# No distinguir entre mayúscula y minúscula
grep A -i /etc/passwd

# Mostrar la cantidad de líneas con la coincidencia
grep A -ic /etc/passwd

# Buscar en todo el directorio
grep root -r /etc/
```

### Expresiones regulares = regex

| Caracter especial | Descripción                                                                             |
| ----------------- | --------------------------------------------------------------------------------------- |
| `^`               | Inicio de línea                                                                         |
| `$`               | Fin de línea                                                                            |
| `.`               | Cualquier carácter.<br>Buscar "punto", delante: \                                       |
| `[aeiou]`         | Pueden aparecer un conjunto determinado de caracteres en una posición. -> `c[aei]s[ao]` |

| Clases         | Descripción                                              |
| -------------- | -------------------------------------------------------- |
| `[ :alnum: ]`  | Letras y dígitos                                         |
| `[ :alpha: ]`  | Letras                                                   |
| `[ :blank: ]`  | Espacios en Blanco                                       |
| `[ :cntrl: ]`  | Caracteres de control                                    |
| `[ :space: ]`  | Los Espacios en Blanco verticales y horizontales         |
| `[ :graph: ]`  | Caracteres imprimibles, sin incluir el Espacio en Blanco |
| `[ :print: ]`  | Caracteres imprimibles, incluyendo el Espacio en Blanco  |
| `[ :digit: ]`  | Dígitos                                                  |
| `[ :lower: ]`  | Letras minúsculas                                        |
| `[ :upper: ]`  | Letras mayúsculas                                        |
| `[ :punct: ]`  | Signos de puntuación                                     |
| `[ :xdigit: ]` | Dígitos Hexadecimales                                    |

Ejemplos:

```bash
# Empieza por "a":
grep ^a /usr/share/dict/american-english| tail

# Termina por "o"
grep o$ /usr/share/dict/american-english| tail

# Palabras que coincidan
grep 'c[aei]s[ao]' /usr/share/dict/british-english

# Encontrar la palabra exacta
grep '^c[aei]s[aeo]$' /usr/share/dict/british-english

# Caracteres genéricos con .
grep '^c[aei]s..[aeiourt]$' /usr/share/dict/british-english

# Contar saltos de linea en todos los archivos
grep ^$ * | wc -l
```

| Repeticiones | Descripción                                                              |
| ------------ | ------------------------------------------------------------------------ |
| X*           | Concuerda con cero o más repeticiones de la regex que le precede (izq)   |
| X?           | Concuerda con cero o una parición de la regex que le precede             |
| X+           | Concuerda con una o más repeticiones de la regex que le precede          |
| X{n}         | Concuerda con n repeticiones exactas de X                                |
| X{n,}        | Concuerda con n o más repeticiones de X                                  |
| X{,n}        | Concuerda con cero o a lo sumo n repeticiones de X                       |
| X{n, m}      | Concuerda con al menos n repeticiones de X, o como mucho m repeticiones. |

Ejemplos:

```bash
# palabra de 8 caracteres
grep -E '^[h-q]{8}$' /usr/share/dict/british-english

# palabra entre 6 y 9 caracteres
grep -E '^[h-q]{6,9}$' /usr/share/dict/british-english

# Mínimo 6 caracteres hasta sin límite
grep -E '^[h-q]{6,}$' /usr/share/dict/british-english

# Máximo hasta 9 caracteres, sin mínimo
grep -E '^[h-q]{,9}$' /usr/share/dict/british-english
```

| Corchetes                  | Descripción                                                                                                           |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Exclusión / Negación `[^]` | Una posición puede encontrarse cualquier carácter **excepto** los que se encuentran entre corchetes = `c[^aei]s[^ao]` |
| Rangos `[-]`               | Indicar todos los valores intermedios entre un inicio y un final.<br> `c[a-d]s[0-5]` == `c[abcd]s[012345]`            |
| Mezcla                     | `[3-8[ :upper: ]mty]` == Admiten números del 3 al 8 o cualquier mayúscula o las minúsculas de m, t, y.                |

### Ordenar y seleccionar líneas únicas

```shell
# Ordenar alfabéticamente o de 0-1 del primer carácter
sort {archivo}

# Al revés
sort -r {archivo}

# Ordenar basado numéricamente
sort -n {archivo}

# Ordenar que lo entienda un humano
sort -h {archivo}

# Líneas de texto únicas entre ellas
uniq {archivo}

# Diferenciar entre mayúscula y minúscula
uniq -i {archivo}

# Textos únicos
uniq -d {archivo}

# Enumerar cuántas veces se repite una línea hasta romperlo
uniq -c {archivo}
```

[Vuelta al índice](#index)

---

## Opciones de configuraciones de red

[GUI - Desktop](https://help.ubuntu.com/community/NetworkManager)
[Server](https://gist.github.com/17twenty/465b813e6219a363a80a)

---

## Configuración por terminal

### Obsoleto

El paquete `net-tools` quedará obsoleto para administrar la red.
Comandos obsoletos:

```shell
# Administrar las interfaces de red
ifconfig

# Muestra las conexiones de red, tablas de encaminamiento y estadísticas de tráfico
netstat

# Administrar las tablas de enrutamiento IP
route
```

### Actual

#### Ping + Traceroute (MTR)

```shell
# Enviar ping
ping {dirección_IP}
# Enviar un cierto número de paquetes
ping -c [num_paquetes] google.com

# Ver los saltos hasta el destino
tracepath {IP}
traceroute {IP}
```

### Comando ip

Sintaxis -> `ip [OPCIONES] OBJECTO [COMANDO [ARGUMENTOS]`

| Objetos   | Descripción                                                                  |
| --------- | ---------------------------------------------------------------------------- |
| link      | Para configurar los objetos físicos o lógicos de la red                      |
| address   | Manejo de direcciones asociadas a los diferentes dispositivos.               |
| neighbour | Administrar los enlaces de vecindad (ARP)                                    |
| rule      | Ver las políticas de enrutado y cambiarlas                                   |
| route     | Ver las tablas de enrutado y cambiar las reglas de las tablas.               |
| tunnel    | Administrar los túneles IP                                                   |
| maddr     | Ver las direcciones multienlace, sus propiedades y cambiarlas                |
| mroute    | Establecer, cambiar o borrar el enrutado multienlace                         |
| monitor   | Monitorizar continuamente el estado de los dispositivos, direcciones y rutas |

Ejemplos:

```shell
# Ayuda del comando
ip --help

# Des/activar interfaz
ip link set {interfaz} up/down

# Des/activar arp
ip link set dev {interfaz} arp on/off

# Ver direcciones IP
ip addr show / ip -c a
ip -c a show {interfaz}

# Añadir/Borrar dirección IP
ip addr add/del {direccion_IP}/{masc_red} dev {interfaz}

# Ver tabla de enrutamiento
ip route show

# Añadir ruta
ip route add {IP_añadir}/{masc_red} via {gateway} dev {interfaz}

# Borrar ruta
ip route del {IP_borrar}

# Establecer puerta de enlace por defecto
ip route add default via {direccion_IP}
```

### Comando ss (Socket Statistics)

Sintaxis -> `ss [options] [FILTER]`

| Opciones | Descripción                                  |
| -------- | -------------------------------------------- |
| -t       | TCP                                          |
| -u       | UDP                                          |
| -l       | Socket a la escucha                          |
| -p       | Muestra el nombre y PID del proceso asociado |
| -s       | Estadísticas resumidas                       |
| -n       | No muestre el nombre web, mostrar solo la IP |

| Filtros               | Descripción                                             |
| --------------------- | ------------------------------------------------------- |
| state                 | Estado del socket                                       |
| exclude               | Se excluye el estado indicado                           |
| EXPRESIÓN             |                                                         |
| operadores            | `and or not`                                            |
| origen y/o destino    | `{src\|dst} [IP[/prefijo][:puerto]`                    |
| puerto origen/destino | `{dport\|sport} {eq\|neq\|gt\|ge\|lt\|le} [IP]: puesto` |

Ejemplos para afinar resultados:

```shell
ss state stablished '(sport = :http or sport = :https)' src {ip}/{masc}
ss sport neq :21 and sport neq :https or not dst {ip}/{masc}
```

### Configuración de red

| Ficheros           | Función                                                                                                                                                                                      |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| /etc/hostname      | Guarda el nombre del host.<br>`hostname {nuevo_nombre}` -> cambiar el nombre provisional<br>`hostname -s` -> consulta el nombre del host<br>`hostname -f` -> consulta el nombre y el dominio |
| /etc/hosts         | Asociar nombres a IPs<br>Cada línea contiene una IP seguido por uno o varios nombres que se asociarán a la IP.                                                                               |
| `hostnamectl`      | Modificar el nombre del host y otros valores.<br>`status` -> ver el estado.<br>`set-hostname {nombre}` -> modificar el nombre permanente.                                                    |
| /etc/resolv.conf   | Fichero en el que se configuran la IP de los servidores DNS para consultar la resolución de nombres.                                                                                         |
| /etc/nsswitch.conf | Se especifica el ordern en el que se buscará información en una serie de fuentes de datos.                                                                                                   |

Ejemplos:

```shell
# Dentro de /etc/hosts, ejemplo de enlazar una IP con un nombre
127.0.0.1 arduriki
# Ahora si le hacemos...
ping arduriki  # ...debería responder

# Dentro de /etc/resolv.conf
nameserver 8.8.8.8  # Un servidor DNS
# Consultar las fuentes DNS
less /etc/nsswitch.conf
```

### Configuración persistente

#### Modelo Debian

```shell
# ver las interfaces de red
ip -c a
# edita la interfaz de red
vim /etc/network/interfaces
# en el archivo, como ejemplo
auto {interfaz_red}
iface {interfaz_red} inet dhcp  # automática
# reiniciar una vez editado el archivo
systemctl restart networking

# Configuración manual en Vim
iface {interfaz_red} inet static
address {IP}
netmask {mascara_completa}
gateway {puerta_enlace}
# reiniciar una vez editado el archivo
systemctl restart networking
# Comprobar las DNS
vim /etc/resolv.conf
# Si el NetworkManager hace de las suyas, desabilitarlo
systemctl stop NetworkManager
```

#### Configuración DNS

```shell
# Ver hosts
cat /etc/hosts

# Ver los ficheros locales y los servidores DNS
getent ahosts {nombre_dns}
```

#### Resolución de DNS

```shell
# Comprobar la configuración de las DNS
cat /run/systemd/resolv.conf

# Si está bien configurado, reiniciar el servicio
systemctl restart systemd-resolved
# Los ve aquí
cat /etc/nsswitch.conf  # En el apartado "hosts"

# Ver la IP y características del dominio
host {web}
# Se le puede añadir el servidor DNS
host {web} {DNS}

# Instalar dnsutils
sudo apt install dnsutils
# Info de la web
dig {web}
# Utilizando un servidor DNS
dig @{DNS} {web}
# Solamente la IP
dig +short google.com
```

[Vuelta al índice](#index)

---

## Seguridad y sistema de permisos de archivos

### Usuarios y grupos

```shell
# Cambio a root para ser administrador del sistema
su
# Cambio a otro usuario
su {usuario}
```

En el comando `ls -l`se pueden observar dos columnas de los documentos y directorios: propietario - grupo

```shell
# Identificar al usuario actual
whoami
# Grupos a los que pertenece
groups
```

Comando `id {usuario}` enseña:

- uid -> id de usuario
- gid -> grupo de usuario

### Cambio de propietario de usuario o grupo

```shell
# Comando "chown" se puede cambiar directamente la propiedad del usuario y del grupo.
chown {usuario}:{grupo} {fichero/carpeta}
# Cambio de grupo
chown :{grupo} {fichero/carpeta}
# CAmbio solo de usuario
chown {usuario} {fichero/carpeta}

# Otra opción para cambiar el grupo
chgrp {grupo} {fichero/carpeta}
# Cambiar el actual usuario a otro grupo que pertenece
newgrp {grupo}
```

### Permisos en ficheros

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones

- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema
Si no aparece la letra `-` significa que no tiene permiso.

## Permisos en directorios

Cuando realizamos un `ls -l`, la primera columna significa:

- `d` -> directorio
- `-` -> archivo regular
- `l` -> link, enlace

En el caso de los directorios los permisos son:

- `r` -> Listar
- `w` -> Modificar los elementos dentro del directorio: borrar, crear, etc.
- `x` -> Entrar en el directorio con `cd`.

### Cambiar permisos: chmod

```shell
# Conceder un permiso de los 3 grupos
chmod +[r/w/x] archivo/directorio
# Quitar permiso de los 3 grupos
chmod -[r/w/x] archivo/directorio
```

Para hacerlo de manera más específica:
`u` -> usuario
`g` -> grupo
`o` -> otro
`a` -> all (todos)

Ejemplo

```shell
# Conceder
chmod [u/g/o]+[r/w/x] {archivo}
# Quitar
chmod [u/g/o]-[r/w/x] {archivo}

# Concatenar permisos
chmod [u/g/o]+[r/w/x],[u/g/o]+[r/w/x] {archivo}  # Importante la coma

# Especificar permisos
chmod [u/g/o]=[r/w/x][r/w/x][r/w/x],[u/g/o]=[r/w/x] {archivo}  # Los permisos no especificados se borran.
```

#### Chmod con notación binario o decimal

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:

- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`

En este tipo de cambio, actuará en los tres grupos.

### Permisos especiales

| Permisos especiales | SetUID (4) | SetGID (2) | Sticky Bit (1) |
| ------------------- | ---------- | ---------- | -------------- |
|                     | 1 / 0      | 1 / 0      | 1 / 0          |

Ejemplos

```shell
# Ejemplo 1: SetUID
# ----------------
# Crear un script que muestre información sensible
cat << 'EOF' > /home/check_shadow
#!/bin/bash
head -n 3 /etc/shadow
EOF

# Dar permisos de ejecución
chmod 755 /home/check_shadow

# Establecer el SetUID y cambiar el propietario a root
chmod u+s /home/check_shadow
chown root:root /home/check_shadow

# Ver los permisos (la 's' en lugar de 'x' indica SetUID)
ls -l /home/check_shadow
# Salida:
-rwsr-xr-x 1 root root 45 Oct 18 10:00 /home/check_shadow
```

```shell
# Ejemplo 2: SetGID
# ----------------
# Crear un directorio compartido para un grupo
mkdir /home/proyecto_grupo
groupadd equipo_desarrollo

# Establecer el grupo propietario
chown :equipo_desarrollo /home/proyecto_grupo

# Aplicar SetGID al directorio
chmod g+s /home/proyecto_grupo

# Ver los permisos (la 's' en la parte del grupo indica SetGID)
ls -ld /home/proyecto_grupo
# Salida: drwxr-sr-x 2 root equipo_desarrollo 4096 Oct 18 10:00 /home/proyecto_grupo

# Probar la herencia de grupo
sudo -u usuario1
touch /home/proyecto_grupo/archivo1.txt
ls -l /home/proyecto_grupo/archivo1.txt
# El archivo heredará automáticamente el grupo equipo_desarrollo

# Verificar bits especiales en formato octal
stat -c '%a %A %n' /home/check_shadow
# Salida ejemplo: 4755 -rwsr-xr-x /home/check_shadow

stat -c '%a %A %n' /home/proyecto_grupo
# Salida ejemplo:
2755 drwxr-sr-x /home/proyecto_grupo
```

```shell
# Ejemplo 3: Sticky Bit
# 1. Crear un directorio de ejemplo 
mkdir /home/shared_folder
chmod 777 /home/shared_folder
# 2. Crear algunos archivos de prueba como diferentes usuarios
sudo -u usuario1
touch /home/shared_folder/archivo1.txt
sudo -u usuario2
touch /home/shared_folder/archivo2.txt
# 3. Ver los permisos actuales
ls -ld /home/shared_folder 
# Salida:
drwxrwxrwx 2 root root 4096 Oct 18 10:00 /home/shared_folder 
# 4. Aplicar el Sticky Bit 
chmod +t /home/shared_folder
# 5. Ver los nuevos permisos
ls -ld /home/shared_folder
# Salida:
drwxrwxrwt 2 root root 4096 Oct 18 10:00 /home/shared_folder
# 6. Intentar eliminar archivos como usuario2
sudo -u usuario2
rm /home/shared_folder/archivo1.txt
# Salida esperada:
rm: cannot remove 'archivo1.txt': Operation not permitted
# 7. El propietario sí puede eliminar su propio archivo
sudo -u usuario2
rm /home/shared_folder/archivo2.txt # Esto sí funcionará
```

### Seguridad en usuarios

`su -` -> cambiar a usuario `root`
`sudo su` -> cambiar el actual usuario a privilegios de superusuario.

#### Logins

`who` -> indica quién está identificado en el sistema.
`w` -> muestra quién hay y qué está ejecutando
`last` -> lista los últimos accesos que ha tenido el sistema.

- También sirve para determinar posibles logeos fallidos y otros comandos
- Si especificamos el usuario, podremos ver las acciones realizadas.

### Administración de usuarios

`/etc/passwd` -> fichero donde se guardarán los usuarios del sistema.

```shell
cat /etc/passwd
# Resultado
usuario:x:UID:GID:datos_personales:directorio_home:shell
```

`useradd [opciones] nombre_usuario` -> Crea un usuario con las opciones por defecto o con las definidas por las opciones:

| Opciones | Descripción             |
| -------- | ----------------------- |
| `-d`     | directorio home         |
| `-m`     | crea el directorio home |
| `-g`     | grupo inicial           |
| `-G`     | grupo/s secundario/s    |
| `s`      | intérprete de comandos  |
| `-k`     | directorio de plantilla |

Los usuarios con `/bin/false` o los `/usr/sbin/nologin` no tienen permiso de un intérprete de comandos.

#### Ejemplo de creación de usuarios

```shell
useradd ana
# Crea el usuario con las opciones por defecto: crea su propio grupo.
# No crea el directorio /home/ana

# Comprobar los grupos disponibles
tail /etc/group

# creación completa
useradd -d /home/delegado -m -g clase -s /bin/bash alex
# comprobar el usuario
id alex

# creación con grupos secundarios
useradd -m -g entrenadores -s /bin/bash -G tenis,padel lucia
```

#### Contraseñas

`/etc/shadow` -> fichero donde se guardarán las contraseñas cifradas de los usuarios y la información referente a su validez.

Posible resultado:
`usuario:contraseña_cifrada($id$salt$hashed):ult_vez_cambiada:min_dias_hasta_volver_cambiarse:max_dias_validez:num_dias_avisara_caducara:dias_caducada_hasta_deshabilitar_cuenta`

##### Ejemplo de contraseñas

```shell
# Establecer contraseñas para los nuevos usuarios como root
passwd ana  # Seguir la guía

# Como deshabilitar un usuario de manera temporal (lock)
passwd -l ana  # Se añade una exclamación en el hash y no se le permitirá el acceso.

# Habilitar al usuario
passwd -u ana
```

### Administración de grupos y usuarios

`/etc/group` -> fichero donde se guardarán los grupos y quienes pertenecen a ellos de forma secundaria.
Ejemplo: `alumnos:x:1002:juan,maria`

1. Nombre del grupo
2. Contraseña en gshadow
3. GID (Group ID)
4. Usuarios que pertenecen al grupo de forma secundaria.

`/etc/skel` -> La plantilla que Linux utiliza cada vez que se crea un nuevo usuario en el sistema. Estos se copiarán automáticamente al directorio home de cada nuevo usuario.
`getent` -> Comando para obtener información sobre usuarios, grupos, contraseñas, etc.

| **Comandos**           | **Descripción**                                                       |
| ---------------------- | --------------------------------------------------------------------- |
| `getent passwd alumno` | línea del fichero del alumno                                          |
| `getent group clase1`  | línea del fichero del grupo                                           |
| `groupdel`             | Borra un grupo                                                        |
| `groupmod`             | Modifica un grupo (el nombre)                                         |
| `usermod`              | Modifica un usuario (como useradd)<br>Añadir al grupo -> `-aG grupo`  |
| `userdel`              | Borra un usuario.<br>Con `-r`elimina sus directorios home y de email. |

### Enlaces en el sistema de ficheros de Linux

Comando `ln`:

- Sin opciones creamos un enlace duro: un puntero a la información del disco.
- Opción `-s`sería un enlace blando: apunta a la ruta

Ejemplos:

```shell
# Crea un enlace fuerte llamado repos que tendrá la misma información que el sources.list
ln /etc/apt/sources.list ~/repos

# Crea un enlace simbolico llamado paquetes, que irá a /var/cache/apt/archives
ln -s /var/cache/apt/archives/ /paquetes/
```

Comprobar el espacio ocupado en el directorio: `ls -lh`

[Vuelta al índice](#index)

---
---
---
