**Nodejs, PostgreSQL & Typescript, REST API CRUD **

Tomado de: https://www.youtube.com/watch?v=z4BNZfZ1Wq8&t=58s

**Vamos a utilizar Visual Studio Code para editar **

Utilizaremos un módulo llamado Pg para hacer consultas básicas a PostGREs
Utiizaremos algunos modulos modernos como await o el try catch para los errores.

**1.Inicializando visual studio code y carpetas **

Creamos una carpeta que se llama nodejs-restapi-postgres-ts.

Arrastramos la carpeta al editor de texto.

Inicializamos la terminal integrada con command shift P.

Escribimos en la terminal:

2. npm init -y  (Inicializa el package.json)

**3. Instalar typescript como dependencia de desarrollo**
 npm i typescript -D  // instala typescript como dependencia de desarrollo, debería aparecer en el json como "devDependencies"

**4. Configurando todas las extensiones necesarias para typescript: nodemon, concurrently**
Creamos carpeta src para diferenciar los archivos de configuración a los archivos del proyecto como tal y creamos index.ts dentro de esa carpeta.
 ponemos  console.log('Hello world')
 Para ejecutar esto necesitamos pasarlo a Javascript, hay varias formas de hacer esto y una es através de npx
 
 tsc= typescript compiler
 
 Escribimos en la terminal:
 
 npx tsc --init //creando archivo de configuración de typescript
 
 En el archivo de configuración de typescript aparece que el target es es5 y lo pasaremos a es6 
 
 Debemos cambiar outDir y rootDir así que los descomentamos
 
 rootDir lo descomentamos y quedará así, conectado a la carpeta source asi:
 "rootDir": "./src",
 
 
 y outDir, donde saldrán los archivos convertidos quedará así, conectado a una carpeta que vamos a crear llamada dist:
 
 "outDir": "./dist",
 
 "strict": true,  se queda descomentado, descomentamos "moduleResolution": "node",  para poder utilizar los modulos de node.
 
  "esModuleInterop": true, se queda descomentado. 
  
 le agregamos al final un exclude, de los archivos que quiero que ignore la carpeta node_modules
 
 ,
  "exclude": ["node_modules"]
  
  ejecutamos npx tsc que convierte los archivos
  
  Se demora un poco y crea la carpeta dist que tiene un index.js con lo que tengamos en el index.ts pero convertido a js
 
Seguimos instalando los paquetes o módulos express y pg // pg es conexión con postgres, driver de conexión

npm i express pg

En consola

Ahora, para requerir estos dos en typescript debería usar la notación de import from...

una forma más rápida de hacer esto es con una extensión de Visual studio code.

ES7 React/Redux/GraphQL/React-Native snippets

Se instala y ya se autocompleta en el index.ts el import... Quedará así:

import express from 'express'
const app = express();

Pero typescript no reconoce los tipos de datos así que en consola escribimos:

npm i @types/express -D

Instalando los tipos de datos de typescript solo para desarrollo. Ya no tenemos el error.

Ahora escribimos abajo:

app.listen(3000, () => {
    console.log('Server on port 3000');
 })
 
 hacemos en consola:
 
 npx tsc 
 
 para convertir el index.ts a index.js
 
 Podemos agregar un script que con el comando build haga npx tss, lo agregamos en el package json
 
 Borramos: "test": "echo \"Error: no test specified\" && exit 1"
 Y agregamos: "build": "tsc"
 
 Ya no pondremos npx tsc sino npx run build

Instalamos un modulo llamado nodemon, que permite reiniciar el servidor cada vez que cambio codigo

Lo hago en la consola así:

npm i nodemon -D

Para usar nodemon, ejecuto un comando que no solo ejecuta los archivos de javascript sino los de typescript así que simplemente agregamos un comando para ejecutar nodemon

Lo agregamos al package.jason asi:

"dev":"nodemon dist/.js" // ejecuta nodemon desde la carpeta dist/index.js

hacemos npm run dev y deberia aparecer al final Server on port 3000

Necesito cambiar los archivos de typescript y nodemon va a estar revisando **la carpeta dist**

Cambiamos tsc por tsc --watch en el package.json

tsc-watch starts a TypeScript compiler with --watch parameter, with the ability to react to compilation status. tsc-watch was created to allow an easy dev process with TypeScript. Commonly used to restart a node server, similar to nodemon but for TypeScrip

Pero necesitaría dos consolas para hacer el tsc --watch y el dev, así que instalaremos un modulo para correr las dos cosas en una sola terminal

Ese modulo se llama concurrently y se instala así:

npm i concurrently -D

Después de eso cambiamos el dev en el package.json para que quede así, la ponemos arriba de "build"

"dev": "concurrently \"tsc --watch\" \"nodemon dist/index.js\""

ahora ponemos 

npm run dev

Entonces a partir de ahora cada vez que guardemos se va a reiniciar el servidor solo....
Ahora, para probar eso, cambiamos en el index.ts el listen con log de callback por una separada asi: (Y cambiamos al port 4000)

import express from 'express'
const app = express();

app.listen(4000);
console.log('Server on port',4000);


Pero necesitaría dos consolas para hacer el tsc --watch y el dev, así que instalaremos un modulo para correr las dos cosas.
**5. Conectando con Postgresql**
Ahora sí abro una nueva consola para trabajar con postgres y hago

sudo -u postgres psql 

-u es para el usuario de postgres
psql es para iniciar el shell de postgres
sudo es para volverse el superusuario de postgres.

Sin embargo en nuestro caso utilizamos: porque postgres corre en deamon mode...

**psql postgres**

Hacemos el comando:

CREATE DATABASE typescriptdatabase;

Vamos a ir escribiendo todos los comandos de postgres en un documento llamado database.sql que va a estar dentro de una carpeta llamada database, que está en la carpeta principal.

Después creamos una tabla así:

\c typescript database;

para entrar la base de datos que he creado 

CREATE TABLE users(
    id SERIAL PRIMARY KEY,  
    name VARCHAR(40),
    email TEXT 
);
Crea la tabla users, la columna id que es na serie de enteros y va a ser la primary key, otra columna llamada name que va a ser strings de máximo 40 y una columna e-mail de string sin limite.

Después agregamos datos así:

INSERT INTO users (name,email)
    VALUES ('joe','joe@ibm.com'),
            ('ryan','ryan@faztweb.com');

\dt para display tables
\d users para mostrar una table, en este caso users

select * from users nos muestra toda la tabla users

Para agregar el dumbfile de arca, se creó una nueva base de datos usando:

CREATE DATABASE databasename

Y se agregó el dumb usando:

\i 'path/to/file.sql'

Para conectarnos a postgres creamos un archivo dentro de la carpeta scr llamado database.ts:

Escribimos, para conectar con postgres:

import {  } from "pg";

Dice que no reconoce pg, entonces vamos a la primera consola y escribimos, para instalar los tipos de datos de pg:

npm i @types/pg -D

Para instalarlos solamente en modo desarrollo

Una vez tengo el módulo, lo que voy a importar desde allí es una clase llamada Pool para poder tener una serie de conexiones para usarlas desde node. Queda así:

import {Pool} from "pg";


Abajo, usamos la función pool para caracterizar el acceso a postgres así:

new Pool ({
user: 'santiagojeda',
host: 'localhost',
password: '271293So',
database: 'typescriptdatabase',
port: 5432,
});












          

6. 
7. 




