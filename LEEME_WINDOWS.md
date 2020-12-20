# Instalación / Configuración entorno PA / 2020-2021 - Windows
-------------------------------------------------------------------------------

## Descargar e instalar el software

> NOTA: Se recomienda utilizar un usuario de Windows sin espacios en el nombre 
  para evitar problemas con Maven.

- Descargar y descomprimir en `C:\Program Files\Java` el siguiente software
    - Maven 3.6.x o superior 
        + https://maven.apache.org/download.cgi
        + Descargar el "Binary zip archive"
	 
- Descargar e instalar AdoptOpenJDK 11
    - https://adoptopenjdk.net/
    - Seleccionar la version "Open JDK 11 (LTS)" y la JVM "Hotspot".
    - Descargar el instalador .msi para Windows e instalar usando las opciones por defecto.

- Descargar e instalar IntelliJ IDEA
    - https://www.jetbrains.com/es-es/idea/download
        + Se puede utilizar la versión Community (libre) o la versión Ultimate 
          (solicitando una licencia para estudiantes). 
    - Instalar usando las opciones por defecto.
	 
- Descargar e instalar MySQL 8:
    - https://dev.mysql.com/downloads/mysql/
        + Descargar el instalador .msi para Windows
    - Instalar en la ruta por defecto.
    - Comprobar que la opción "Start the MySQL Server at System Startup"
      está marcada, para que se instale como servicio Windows.
    - Elegir "Server only" o "Custom" (para instalar Server + Workbench) y usar 
     las opciones por defecto.
    - Después de la instalación, se ejecutará el wizard de Configuración de 
     MySQL Server. Utilizar las opciones por defecto excepto las siguientes:
         + Debe introducirse una contraseña no vacía para el usuario `root` (e.g. `root`)

- Node.js 14 (LTS version)
    - Descargar el instalador de https://nodejs.org/es/download/
    - Doble-click en el instalador e instalar con las opciones por defecto

## Descargar y descomprimir pa-shop de Moodle
- Descargar y descomprimir en `C:\software`

## Establecer variables de entorno

- Ir a "Panel de Control > Sistema > Configuración avanzada del sistema > Variables de entorno ..."

- En la sección "Variables de usuario para `<user>`", crear las siguientes
  variables de entorno (para cada una pulsar en "Nueva ...", introducir el 
  nombre y el valor, y pulsar "Aceptar")
    - Nombre: `JAVA_HOME`
        + Valor: Directorio donde se instaló AdoptOpenJDK
        + Por ejemplo:`C:\Program Files\AdoptOpenJDK\jdk-11.0.8.10-hotspot`
    - Nombre: `MAVEN_HOME`
        + Valor: Directorio donde se descomprimió Maven
        + Por ejemplo: `C:\Program Files\Java\apache-maven-3.6.3`
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

## Creación de bases de datos necesarias para los ejemplos

- Arrancar MySQL
  - Si se ha instalado como servicio seguramente se haya iniciado de forma 
    automática. En otro caso habría que iniciar el servicio manualmente.
    
> NOTA: En Panel de Control, Servicios Locales se puede configurar arranque 
  automático o manual. También se puede arrancar y detener.
           
> NOTA: Cuando se ejecutan los comandos `mysqladmin` y `myqsl`, con la opción
  `-p` se nos solicitará la password del usuario root.

- Creación de bases de datos ws y wstest (abrir en una consola diferente)

```shell
	mysqladmin -u root create pa -p
	mysqladmin -u root create patest -p
	mysqladmin -u root create paproject -p
	mysqladmin -u root create paprojecttest -p    
```

- Creación de usuario ws con password con permisos sobre ws y wstest

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

## Inicialización de datos de ejemplo y compilación de los ejemplos

- Inicialización de la base de datos y compilación/configuración de los ejemplos

```shell
    cd \software\pa-shop-<version>/backend
    mvn sql:execute install
    cd \software\pa-shop-<version>/frontend
    npm install
```

  
## Instalación y configuración básica de Git
> NOTA: Este paso no es necesario si ya se utilizó y configuró Git en otras asignaturas

- Descargar e instalar Git
    - https://git-scm.com/downloads
    - Hacer clic en "Windows" para descargar.
    - Instalar con las opciones por defecto.

- Configuración básica

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

### Creación y configuración de claves SSH
> NOTA: Este paso no es necesario si ya utilizó Git en otras asignaturas

- Desde el intérprete de comandos git-bash ejecutar:

> Genera las claves en la ruta por defecto (%USERPROFILE%/.ssh) y con los nombres  por defecto 
      
```shell
    ssh-keygen -t rsa -b 4096 -C "your_email@udc.es"
```    
    
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
