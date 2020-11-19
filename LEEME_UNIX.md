# Instalación / Configuración entorno PA / 2020-2021 - Linux y macOS


## Descargar e instalar el software
  
- [Linux] 
    - Descargar y descomprimir en `$HOME/software` el siguiente software
        - Maven 3.6.x o superior 
            - https://maven.apache.org/download.cgi
            - Descargar el "Binary tar.gz archive".
        - IntelliJ IDEA
            - https://www.jetbrains.com/es-es/idea/download
            - Se recomienda descargar la versión Ultimate. Solicitar la licencia según se indica en https://www.jetbrains.com/es-es/community/education/#students. Si se descarga la versión Community (no requiere licencia), la edición de código del frontend (JavaScript) deberá realizarse con otro editor (e.g. Visual Studio Code).
        - Node 14 LTS
            - https://nodejs.org.
            - Descargar la distribución Descargar la distribución binaria .tar.xz (se descomprime usando `tar zxvf <<fichero>>`).
    - Instalar como paquete
        - AdoptOpenJDK 11
            - Instalar la versión "adoptopenjdk-11-hotspot" siguiendo las instrucciones que se indican en https://adoptopenjdk.net/installation.html#linux-pkg.
        - MySQL 8
            - Para Debian y Ubuntu seguir las instrucciones que se indican en https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/#apt-repo-fresh-install.
            - Para otras distribuciones de Linux, seguir las instrucciones que se indican en https://dev.mysql.com/doc/refman/8.0/en/linux-installation.html.
            
- [macOS] 
    - Descargar y descomprimir en `$HOME/software`
        - Maven 3.6.x o superior 
            - https://maven.apache.org/download.cgi
            - Descargar el "Binary tar.gz archive"
    - Descargar e instalar
        - IntelliJ IDEA
            - https://www.jetbrains.com/es-es/idea/download
            - Se recomienda descargar la versión Ultimate. Solicitar la licencia según se indica en https://www.jetbrains.com/es-es/community/education/#students. Si se descarga la versión Community (no requiere licencia), la edición de código del frontend (JavaScript) deberá realizarse con otro editor (e.g. Visual Studio Code).
            - Descargar y ejecutar el instalador .dmg.
        - AdoptOpenJDK 11
            - https://adoptopenjdk.net/
            - Seleccionar la version "Open JDK 11 (LTS)" y la JVM "Hotspot".
            - Descargar el instalador .pkg e instalar usando las opciones por defecto.
        - MySQL 8
            - https://dev.mysql.com/downloads/mysql/
            - Descargar el instalador .dmg e instalar usando las opciones por defecto.
            - Preferencias del sistema -> MySQL -> Elegir "Start MySQL when your computer starts up".
            - Más información: https://dev.mysql.com/doc/refman/8.0/en/osx-installation.html.
        - Node 14 LTS
            - https://nodejs.org.
            - Descargar y ejecutar el instalador .pkg.
         
## Descargar y descomprimir pa-shop de Moodle
- Descargar en `$HOME/software`

```shell
    cd $HOME/software
    unzip pa-shop-<version>.zip
```

## [Linux] Establecer variables de entorno
- Añadir al fichero `$HOME/.bashrc` lo siguiente 

> NOTA: Los valores de las variables JAVA_HOME, MAVEN_HOME, NODE_HOME e IDEA_HOME deben sustituirse por los directorios donde se haya instalado AdoptOpenJDK y descomprimido Maven, Node and IntelliJ IDEA, respectivamente.

```shell
    # AdoptOpenJDK (Linux)
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

    PATH=$JAVA_HOME/bin:$PATH

    # Maven
    MAVEN_HOME=$HOME/software/apache-maven-3.6.3
    PATH=$MAVEN_HOME/bin:$PATH
    export MAVEN_OPTS="-Xms512m -Xmx1024m"

    # Node.
    NODE_HOME=$HOME/software/node-v14.15.0-linux-x64
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
    # AdoptOpenJDK (macOS)
    export JAVA_HOME=/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home
    PATH=$JAVA_HOME/bin:$PATH

    # Maven
    MAVEN_HOME=$HOME/software/apache-maven-3.6.3
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
    
## Creación de bases de datos necesarias para los ejemplos

- Arrancar MySQL (sólo si el arranque no es automático)

```shell
    mysqld
```

> NOTA: En los siguientes pasos, al ejecutar los comandos  `mysqladmin` y `myqsl` con la opción `-p`, la password que se nos solicitará es la del usuario root que se especificó al instalar MySQL.

- Creación de las bases de datos "pa", "patest", "paproject" y "paprojecttest" (abrir en una consola diferente)

```shell
  mysqladmin -u root create pa -p
  mysqladmin -u root create patest -p
  mysqladmin -u root create paproject -p
  mysqladmin -u root create paprojecttest -p
```

- Creación de usuario "pa" con password "pa" y con permisos sobre las bases de datos "pa", "patest", "paproject" y "paprojecttest".

```shell
    mysql -u root
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
        
## Inicialización de datos de ejemplo y compilación de los ejemplos

- Inicialización de la base de datos y compilación de los ejemplos

```shell
    cd $HOME/software/pa-shop-<version>/backend
    mvn sql:execute package
    cd $HOME/software/pa-shop-<version>/frontend
    npm install
```
    
## Finalizar la ejecución de la BD

- Finalizar la ejecución de la BD (sólo si el arranque no es automático)

```shell
    mysqladmin -u root shutdown
```
      
## Instalación y configuración básica de Git
> NOTA: Este paso no es necesario si ya utilizó Git en otras asignaturas.

- Instalación en Linux
    - https://git-scm.com/downloads
    - Hacer clic en "Linux/Unix" y seguir las instrucciones según la distribución de linux utilizada.
     
- Instalación en macOS
    - https://git-scm.com/downloads
    - Hacer clic en "Mac OS X". En la siguiente pantalla, dentro de la sección "Binary Installer",
      bajar la última versión disponible (un fichero .dmg).
    - Instalar con las opciones por defecto.

- Configuración básica (Linux y macOS)

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
> NOTA: Este paso no es necesario si ya utilizó Git en otras asignaturas.

- Desde un terminal ejecutar:

> Generar las claves en la ruta por defecto ($HOME/.ssh) y con los nombres 
  por defecto
       
```shell
    ssh-keygen -t rsa -b 4096 -C "your_email@udc.es"
```

- Acceder a [https://git.fic.udc.es/profile/keys](https://git.fic.udc.es/profile/keys)
- En el campo "Key" copiar la clave pública, es decir, el contenido del fichero 
  `$HOME/.ssh/id_rsa.pub`
- En el campo "Title" ponerle un nombre
- Clic en "Add key"

- Comprobar conexión SSH con el servidor de git y añadirlo a la lista de hosts 
  conocidos 
  
> Contestar "yes" a "Are you sure you want to continue connecting (yes/no)?"
    
```shell
    ssh -T git@git.fic.udc.es
```

