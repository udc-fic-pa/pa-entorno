# Instalación / Configuración entorno PA / Windows

## Descargar e instalar el software

> NOTA: Se recomienda utilizar un usuario de Windows sin espacios en el nombre 
  para evitar problemas con Maven.

- Descargar y descomprimir Maven 3.8.x o superior en `C:\software`
    + https://maven.apache.org/download.cgi
    + Descargar el "Binary zip archive"
	 
- Descargar e instalar la última versión LTS de Eclipse Temurin (JDK 17)
    - https://adoptium.net
    - Descargar el instalador .msi para Windows e instalar usando las opciones por defecto.

- Descargar e instalar IntelliJ IDEA
    - https://www.jetbrains.com/es-es/idea/download
    - Se recomienda descargar la versión Ultimate. Solicitar la licencia según se indica en 
      https://www.jetbrains.com/es-es/community/education/#students. Si se descarga la versión 
      Community (no requiere licencia), la edición de código del frontend (JavaScript) deberá 
      realizarse con otro editor (e.g. Visual Studio Code).
    - Instalar usando las opciones por defecto.
    - Configuración:
      - Arrancar IDEA.
      - File -> New Projects Setup -> Settings for New Projects -> Editor -> File Encodings -> Properties Files (*.properties) -> Default encoding for properties files -> UTF-8.
	 
- Descargar e instalar MySQL 8.0.x:
    - https://dev.mysql.com/downloads/mysql/
        + Descargar el instalador .msi para Windows de la versión 8.0.x.
    - Instalar en la ruta por defecto.
    - Comprobar que la opción "Start the MySQL Server at System Startup"
      está marcada, para que se instale como servicio Windows.
    - Elegir "Server only" o "Custom" (para instalar Server + Workbench) y usar 
     las opciones por defecto.
    - Después de la instalación, se ejecutará el wizard de Configuración de 
     MySQL Server. Utilizar las opciones por defecto excepto las siguientes:
         + Debe introducirse una contraseña no vacía para el usuario `root` (e.g. `root`)

- Node.js 18 LTS
    - Descargar el instalador: https://nodejs.org/download/release/v18.16.0/node-v18.16.0-x64.msi.
    - Doble-clic en el instalador e instalar con las opciones por defecto.

- Git
    - https://git-scm.com/downloads
    - Hacer clic en "Windows" para descargar.
    - Instalar con las opciones por defecto.

## Clonar pa-shop

```shell
    cd c:\software
    git clone git@github.com:udc-fic-pa/pa-shop.git
```

## Establecer variables de entorno

- Ir a "Panel de Control > Sistema > Configuración avanzada del sistema > Variables de entorno ..."

- En la sección "Variables de usuario para `<user>`", crear las siguientes
  variables de entorno (para cada una pulsar en "Nueva ...", introducir el 
  nombre y el valor, y pulsar "Aceptar")
    - Nombre: `JAVA_HOME`
        + Valor: Directorio donde se instaló AdoptOpenJDK
        + Por ejemplo:`C:\Program Files\Eclipse Adoptium\jdk-17.0.3.7-hotspot`
    - Nombre: `MAVEN_HOME`
        + Valor: Directorio donde se descomprimió Maven
        + Por ejemplo: `C:\software\apache-maven-3.8.5`
    - Nombre: `MAVEN_OPTS`
        + Valor: `-Xms512m -Xmx1024m`
    - Nombre: `MYSQL_HOME`
        + Valor: Directorio donde se instaló MySQL
        + Por ejemplo: `C:\Program Files\MySQL\MySQL Server 8.0`

- En la sección "Variables de usuario para `<user>`", modificar la variable de
  entorno `PATH`. Para ello hay que seleccionarla, pulsar en "Editar..." y 
  añadir al principio de su valor (sin borrar su valor antiguo):
  
  `%JAVA_HOME%\bin;%MAVEN_HOME%\bin;%MYSQL_HOME%\bin;`
  
> NOTA: Si la variable de entorno PATH no existiese, entonces habría que 
    crearla procediendo de igual forma que se hizo con las variables anteriores.
    
- Cerrar todos los terminales y abrir terminales nuevos

- Comprobar que el entorno ha quedado correctamente configurado comprobando 
  salidas de los siguientes comandos:
  
```shell 
    java -version
    mvn -version
    node -v
    mysqld --version
```

- Comprobar que se puede ejecutar IntelliJ IDEA

## Creación de bases de datos necesarias para pa-shop y la práctica

- Arrancar MySQL
  - Si se ha instalado como servicio seguramente se haya iniciado de forma 
    automática. En otro caso habría que iniciar el servicio manualmente.
    
> NOTA 1: En Panel de Control, Servicios Locales se puede configurar arranque 
  automático o manual. También se puede arrancar y detener.
           
> NOTA 2: Cuando se ejecutan los comandos `mysqladmin` y `myqsl` con la opción
  `-p`, la password que se nos solicitará es la del usuario root que se especificó al instalar MySQL.

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

- Comprobar acceso a las bases de datos

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
    cd \software\pa-shop\backend
    mvn sql:execute install
    cd \software\pa-shop\frontend
    npm install
```

  
## Configuración básica de Git
> NOTA: Este paso no es necesario si ya se utilizó y configuró Git en otras asignaturas

> NOTA: `$GIT_HOME` debe sustituirse por la ruta donde se instaló git.

- Ejecutar git-bash (`$GIT_HOME/git-bash.exe`) y desde ese intérprete de comandos ejecutar:
    
```shell
    git config --global user.email "your_email@udc.es"
    git config --global user.name "Your Name"
```

> El siguiente comando ilustra como configurar Sublime como editor por defecto de Git, aunque se puede utilizar otro editor instalado en el sistema operativo.
    
```shell
    git config --global core.editor "'C:\Program Files\Sublime Text 3\sublime_text.exe' -w"
```

## Creación y configuración de claves SSH
> NOTA: Este paso no es necesario si ya se utilizó Git con SSH en otras asignaturas

- Desde el intérprete de comandos git-bash ejecutar:

> Genera las claves en la ruta por defecto (%USERPROFILE%/.ssh) y con los nombres  por defecto 
      
```shell
    ssh-keygen -t rsa -b 4096 -C "your_email@udc.es"
```    

## Añadir clave SSH a GitHub
> NOTA: Este paso no es necesario si ya se utilizó GitHub con SSH en otras asignaturas.
    
- Acceder a [https://github.com/settings/keys](https://github.com/settings/keys).
- Añadir una clave SSH.
- En el campo "Title" ponerle un nombre.
- En el campo "Key" copiar la clave pública, es decir, el contenido del fichero 
  `%USERPROFILE%/.ssh/id_rsa.pub`
- Clic en "Add SSH key".

- Comprobar conexión SSH con el servidor de git y añadirlo a la lista de hosts conocidos. Desde git-bash:
  
> Contestar "yes" a "Are you sure you want to continue connecting (yes/no)?"
   
```shell
    ssh -T git@github.com
```
