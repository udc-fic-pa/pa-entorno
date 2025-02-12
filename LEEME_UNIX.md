# Instalación / Configuración entorno PA / Linux y macOS

## Descargar e instalar el software
  
- [Linux] 
    - Descargar y descomprimir en `$HOME/software` el siguiente software
        - Última versión LTS de Eclipse Temurin (JDK 21)
            - https://adoptium.net
            - Descargar el arhivo .tar.gz.
        - Maven 3.9.x o superior 
            - https://maven.apache.org/download.cgi
            - Descargar el "Binary tar.gz archive".
        - IntelliJ IDEA
            - https://www.jetbrains.com/es-es/idea/download
            - Se recomienda descargar la versión Ultimate. Solicitar la licencia según se indica en https://www.jetbrains.com/es-es/community/education/#students. Si se descarga la versión Community (no requiere licencia), la edición de código del frontend (JavaScript) deberá realizarse con otro editor (e.g. Visual Studio Code).
            - Configuración:
              - Arrancar IDEA.
              - File -> New Projects Setup -> Settings for New Projects -> Editor -> File Encodings -> Properties Files (*.properties) -> Default encoding for properties files -> UTF-8.

        - Node.js 20 LTS
            - Descargar la distribución binaria .tar.xz: https://nodejs.org/download/release/v20.11.1/node-v20.11.1-linux-x64.tar.xz.
            - Descomprimirla usando `tar -Jxvf <<fichero>>`.
    - Instalar como paquete
        - MySQL 8.4.x
            - Para Debian y Ubuntu, seguir las instrucciones que se indican en https://dev.mysql.com/doc/refman/8.4/en/linux-installation-apt-repo.html para instalar la versión 8.4.x.
            - Para otras distribuciones de Linux, seguir las instrucciones que se indican en https://dev.mysql.com/doc/refman/8.4/en/linux-installation.html para instalar la versión 8.4.x.
        - Git
            - https://git-scm.com/downloads
            - Hacer clic en "Linux/Unix" y seguir las instrucciones según la distribución de linux utilizada.
            
- [macOS] 
    - Descargar y descomprimir en `$HOME/software`
        - Maven 3.9.x o superior 
            - https://maven.apache.org/download.cgi
            - Descargar el "Binary tar.gz archive"
    - Descargar e instalar
        - IntelliJ IDEA
            - https://www.jetbrains.com/es-es/idea/download
            - Se recomienda descargar la versión Ultimate. Solicitar la licencia según se indica en https://www.jetbrains.com/es-es/community/education/#students. Si se descarga la versión Community (no requiere licencia), la edición de código del frontend (JavaScript) deberá realizarse con otro editor (e.g. Visual Studio Code).
            - Descargar y ejecutar el instalador .dmg.
            - Configuración:
              - Arrancar IDEA.
              - File -> New Projects Setup -> Preferences for New Projects -> Editor -> File Encodings -> Properties Files (*.properties) -> Default encoding for properties files -> UTF-8.
        - Última versión LTS de Eclipse Temurin (JDK 21)
            - https://adoptium.net
            - Descargar el instalador .pkg e instalar usando las opciones por defecto.
        - MySQL 8.4.x
            - https://dev.mysql.com/downloads/mysql/
            - Descargar el instalador .dmg para la versión 8.4.x e instalar usando las opciones por defecto.
            - Preferencias del sistema -> MySQL -> Elegir "Start MySQL when your computer starts up".
            - Más información: https://dev.mysql.com/doc/refman/8.4/en/macos-installation.html.
        - Node.js 20 LTS
            - Descargar el instalador: https://nodejs.org/download/release/v20.11.1/node-v20.11.1.pkg.
            - Doble-clic en el instalador.
        - Git
            - https://git-scm.com/downloads
            - Hacer clic en "Mac OS X". En la siguiente pantalla, dentro de la sección "Binary Installer",
              bajar la última versión disponible (un fichero .dmg).
            - Instalar con las opciones por defecto.
         
## Clonar pa-shop

```shell
    cd $HOME/software
    git clone git@github.com:udc-fic-pa/pa-shop.git
```

## [Linux] Establecer variables de entorno
- Añadir al fichero `$HOME/.bashrc` lo siguiente 

> NOTA: Los valores de las variables JAVA_HOME, MAVEN_HOME, NODE_HOME e IDEA_HOME deben sustituirse por los directorios donde se haya instalado AdoptOpenJDK y descomprimido Maven, Node and IntelliJ IDEA, respectivamente.

```shell
    # Eclipse Temurin
    export JAVA_HOME=$HOME/software/jdk-21.0.2+13
    PATH=$JAVA_HOME/bin:$PATH

    # Maven
    MAVEN_HOME=$HOME/software/apache-maven-3.9.4
    PATH=$MAVEN_HOME/bin:$PATH
    export MAVEN_OPTS="-Xms512m -Xmx1024m"

    # Node.
    NODE_HOME=$HOME/software/node-v20.11.1-linux-x64
    PATH=$NODE_HOME/bin:$PATH

    # IntelliJ IDEA
    IDEA_HOME=$HOME/software/idea
    PATH=$IDEA_HOME/bin:$PATH
```

- Cerrar todos los terminales y abrir terminales nuevos

- Comprobar que el entorno ha quedado correctamente configurado comprobando las salidas de los siguientes comandos

```shell
    which java
    which mvn
    which mysql
    which node
    which idea.sh
```

## [macOS] Establecer variables de entorno
> NOTA: asumiendo que la aplicación Terminal use el shell de inicio de sesión, éste será "zsh" (macOS 10.15 o superior) o "bash" (versiones anteriores de macOS).

- Añadir al fichero `$HOME/.zshrc` (macOS 10.15 o superior) o 
  `$HOME/.bash_profile` (versiones anteriores de macOS) lo siguiente:

> NOTA: Los valores de las variables JAVA_HOME y MAVEN_HOME deben sustituirse por los directorios donde se haya instalado AdoptOpenJDK y descomprimido Maven, respectivamente.

```shell
    # Eclipse Temurin
    export JAVA_HOME=/Library/Java/JavaVirtualMachines/temurin-21.jdk/Contents/Home
    PATH=$JAVA_HOME/bin:$PATH

    # Maven
    MAVEN_HOME=$HOME/software/apache-maven-3.9.4
    PATH=$MAVEN_HOME/bin:$PATH
    export MAVEN_OPTS="-Xms512m -Xmx1024m"

    # MySQL.
    MYSQL_HOME=/usr/local/mysql
    PATH=$MYSQL_HOME/bin:$PATH
```

- Cerrar todos los terminales y abrir terminales nuevos

- Comprobar que el entorno ha quedado correctamente configurado comprobando 
  las salidas de los siguientes comandos

```shell
    which java
    which mvn
    which mysql
    which node
```
    
## Creación de bases de datos necesarias para pa-shop y la práctica

- Arrancar MySQL (sólo si el arranque no es automático)

```shell
    mysqld
```

> NOTA 1: En los siguientes pasos, al ejecutar los comandos  `mysqladmin` y `myqsl` con la opción `-p`, la password que se nos solicitará es la del usuario root que se especificó al instalar MySQL.

> NOTA 2: Si el instalador de MySQL en Linux no nos permitió configurar la contraseña del usuario `root` durante la instalación, los siguientes
comandos deben ejecutarse con el comando `sudo` delante para que no se nos solicite la contraseña del usuario `root` de MySQL.

- Creación de las bases de datos "pa", "patest", "paproject" y "paprojecttest" (abrir en una consola diferente)

```shell
  mysqladmin -u root create pa -p
  mysqladmin -u root create patest -p
  mysqladmin -u root create paproject -p
  mysqladmin -u root create paprojecttest -p
```

- Creación de usuario "pa" con password "pa" y con permisos sobre las bases de datos "pa", "patest", "paproject" y "paprojecttest".

```shell
    mysql -u root -p
        CREATE USER 'pa'@'localhost' IDENTIFIED BY 'pa';
        GRANT ALL PRIVILEGES ON pa.* to 'pa'@'localhost' WITH GRANT OPTION;
        GRANT ALL PRIVILEGES ON patest.* to 'pa'@'localhost' WITH GRANT OPTION;
        GRANT ALL PRIVILEGES ON paproject.* to 'pa'@'localhost' WITH GRANT OPTION;
        GRANT ALL PRIVILEGES ON paprojecttest.* to 'pa'@'localhost' WITH GRANT OPTION;
        exit
```

- Comprobar acceso a las bases de datos:

```shell
    mysql -u pa --password=pa pa
        exit

    mysql -u pa --password=pa patest
        exit

    mysql -u pa --password=pa paproject
        exit

    mysql -u pa --password=pa paprojecttest
        exit
```
        
## Inicialización de datos de ejemplo y compilación de pa-shop

- Inicialización de la base de datos y compilación de pa-shop

```shell
    cd $HOME/software/pa-shop/backend
    mvn sql:execute package
    cd $HOME/software/pa-shop/frontend
    npm install
```
    
## Finalizar la ejecución de la BD

- Finalizar la ejecución de la BD (sólo si el arranque no es automático)

```shell
    mysqladmin -u root shutdown
```
      
## Configuración básica de Git
> NOTA: Este paso no es necesario si ya se utilizó y configuró Git en otras asignaturas.

```shell
    git config --global user.email "your_email@udc.es"
    git config --global user.name "Your Name"
```

> Los siguientes ejemplos ilustran como configurar Code, Atom o Sublime como 
editor de Git (por defecto, se usa "vi"), aunque se puede utilizar otro editor 
instalado en el sistema operativo.

```shell
    git config --global core.editor "code --wait"
    git config --global core.editor "atom --wait"
    git config --global core.editor "subl -n -w"
```

## Creación y configuración de claves SSH
> NOTA: Este paso no es necesario si ya se utilizó Git con SSH en otras asignaturas.

- Desde un terminal ejecutar:

> Generar las claves en la ruta por defecto ($HOME/.ssh) y con los nombres 
  por defecto
       
```shell
    ssh-keygen -t rsa -b 4096 -C "your_email@udc.es"
```

## Añadir clave SSH a GitHub
> NOTA: Este paso no es necesario si ya se utilizó GitHub con SSH en otras asignaturas.

- Acceder a [https://github.com/settings/keys](https://github.com/settings/keys).
- Añadir una clave SSH.
- En el campo "Title" ponerle un nombre.
- En el campo "Key" copiar la clave pública, es decir, el contenido del fichero 
  `$HOME/.ssh/id_rsa.pub`
- Clic en "Add SSH key".

- Comprobar conexión SSH con el servidor de git y añadirlo a la lista de hosts conocidos. 
  
> Contestar "yes" a "Are you sure you want to continue connecting (yes/no)?"
    
```shell
    ssh -T git@github.com
```

