# Creación de un servidor con AzerothCore en Debian 10.

En esta guía, me baso en la información proporcionada por la wiki oficial de Azeroth, quizás haciendo algunas pequeñas modificaciones y por supuesto, totalmente en español. Todo lo que se explique en este apartado, tratare de mantenerlo actualizado, pero a medida que vayan actualizándose las librerías, quizás algo termine volviéndose obsoleto. Si detectan algun error, por favor avísenme para poder corregirlo.

Lo primero es ingresar como **root**, o un usuario con **sudo** para instalar las dependencias.

 - Con root:
```bash
apt-get update && apt-get install git cmake make gcc g++ clang default-libmysqlclient-dev libssl-dev libbz2-dev libreadline-dev libncurses-dev mariadb-server libace-6.* libace-dev
```

 - Con usuario sudo:
```bash
sudo apt-get update && sudo apt-get install git cmake make gcc g++ clang default-libmysqlclient-dev libssl-dev libbz2-dev libreadline-dev libncurses-dev mariadb-server libace-6.* libace-dev
```

Vamos a explicar brevemente que hacen esas líneas, la primera de todas `apt-get update` actualiza la lista de repositorio de debian, revisando y dejando a disposición de las últimas versiones de las librerías. Con `&&` es ejecutar ambos lados de la expresión al mismo tiempo, es decir, cuando finaliza el update, entonces que se ponga a hacer la instalación de las dependencias en este caso.

Una vez, instalas las dependencias de manera correcta, recomiendo no utilizar el usuario `root` para el proyecto, dado que dicho usuario, es el administrador total de nuestro sistema operativo, es preferible crear un usuario para el servidor, darle permisos de `sudo` o, antes de ejecutar el servidor, configurar para que los procesos de dicho usuario tengan una prioridad superior a la normal. (Ya hablaremos del comando más adelante).

En Linux, para crear un usuario, tenemos 2 comandos, yo normalmente utilizo el siguiente:

```bash
adduser <nombre>
```

Siendo `<nombre>` el usuario a crear.
No debemos incluir `<>`. Por ejemplo.

```bash
adduser azerothwow
```

Nos hará un par de preguntas, la primera de ella es la contraseña y luego preguntas opcionales que simplemente podemos ir salteando sin la necesidad de completarlas (Presionando la tecla enter). Nos creara una carpeta de usuario, ubicada dentro del directorio `/home` con nuestro nombre de usuario, pero no se preocupen, que no lo vamos a necesitar, debido a que contamos con la variable de entorno `$HOME` que almacena en todo momento, la ruta a nuestra carpeta de usuario (el usuario que tenga la sesión activa)

Ya no tenemos que hacer más nada con nuestro usuario `root` en caso de que hayamos usado el mismo para instalar las dependencias. NOTA: si realizaste la instalación de las dependencias con un usuario `sudo` no es necesario que quizás crees un nuevo usuario, aunque sería recomendable, para tener las cosas relacionadas al wow, en una carpeta totalmente independiente.

Bien, ahora tenemos que descargar el repositorio, lo vamos a hacer mediante `git`

En la guía original, menciona 3 opciones de clonación, yo me voy a centrar en la opción número 1, que es la recomendada, dado que solamente estamos descargando la rama MASTER que es la de producción, que es estable y a que los desarrolladores, aportan código mediante pull request (solicitudes de cambios)

```bash
git clone https://github.com/azerothcore/azerothcore-wotlk.git --branch master --single-branch azerothcore
```
