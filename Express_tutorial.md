**Express es un framework de node.**
Sirve para escribir aplicaciones profesionales en node.js con pocas líneas de código. 
Requiere:
--> Front End Basics:
  HTML
  CSS
  Javascript Basics
--> Node.js
  npm Basics
(Optional) MVC Basics

Comenzando con MVC 

Model view Controller 

![image](https://user-images.githubusercontent.com/69771095/113515351-a3e06a00-9539-11eb-8394-304433daf6c0.png)

![image](https://user-images.githubusercontent.com/69771095/113515384-ca060a00-9539-11eb-8051-0d50fc7ff8b4.png)

MVC es un patrón de diseño.
Es una arquitectura para una aplicación, la manera en la que funciona una aplicación.
El usuario interactúa con la ventana, que es la vista (View) que interactúa con el controlador. El código para que funcione la ventana es el controlador.

Se separa una aplicación en dos lógicas:
User interface
Business Logic: Es para que funcione la interfaz. 

El patrón de diseño separa la lógica de una aplicación. La lógica de la interfaz es la view.

MVC Frameworks

Ruby
Ruby on rails
Sinatra

Php
CakePhp
Zend
CodeIgniter
Laravel


Python
Django
Flask

JS
express
sails
Backbone
Angular
Net
ASP MVC


How the web works?

User flow:

**VIEW**
CLIENT:
Browsers
HTML/CSS/JS
Just speaks with the controller. 

**CONTROLLER**
SERVER:
Linux/Windows Server
Server side PRogramming Languages
Parte intermedia
Process Request HTTP(GET,PUT,POST,DELETE)
Gets data to the model, Pass data to the view.

**Model**
El controlador guarda los datos aquí. 
DATABASE:
SQL Mysql /NOSQL MongoDB
Store all data. Escrito en SQL, Data related logic. Communicates with controller. **Can update the views depending on the framework** 
Database (SELECT INSERT UPDATE DELETE).... Proceso de manipulación de datos. 


**Este curso de express, node.js framework es para crear el CONTROLLER**

Express en un framework contruido encima de node.js, en realidad es un módulo de node.js que puedes instalar. You can write MVC Apps. 
MEAN y MERN 

Express: Fast, unopinionated, minimalist.

**Create our first server: "Hello world with node and express"**

Node.js dependency
node --version
npm --version
  npm i express

Express generator
npm install -g express
express commands: -h

Yeoman, a web application generator
npm install -g yo

Abrimos el editor de código (Atom o Visual Studio Code) y abrimos el terminal. 

Arrastramos la carpeta al editor de código. 

Dentro de la carpeta ponemos npm init --yes

Esto va a crear un package.json que se salta el cuestionario, así: 

{
  "name": "primer-server-en-express",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

Creamos el archivo index.js

En index.js creamos:

const http = require('http');

Y creamos el servidor:

http.createServer();


**Arrow function breakdown:**

// Traditional Function
function (a){
  return a + 100;
}

// Arrow Function Break Down

// 1. Remove the word "function" and place arrow between the argument and opening body bracket
(a) => {
  return a + 100;
}

// 2. Remove the body brackets and word "return" -- the return is implied.
(a) => a + 100;

// 3. Remove the argument parentheses
a => a + 100;

Al final nos quedará así: 

const http = require('http');

http.createServer((req,res) => { // Igual a function(req,res) {}//
res.end('Hello World');
}).listen(3000);

Ahora instalamos express:

npm install express

Esto crea la carpeta node_modules en el proyecto y el packagelock.json que lleva la descripción de los módulos instalados. 

Vamos a crear el servidor en express: (Se puede borrar el requerimiento de http)


const express= require('express');
const app= express(); // inicializa la función inicial de express. 

app.listen(3000); //.listen creates a listener in a a specific path

Al correr el código así, debería aparecer: 

Cannot GET /

En el navegador, porque no le hemos asignado nada cuando pase get. 

app.get() //Cuando el servidor reciba una petición get, como en el caso de un navegador, cuando un navegador hace una petición get que es de obtener algo. 

// respond with "hello world" when a GET request is made to the homepage ('/' es homepage)
app.get('/', function (req, res) {
  res.send('hello world')
})

Quedará después:

const express= require('express');
const app= express();


app.get('/',(req,res) => { // cuando recibamos una petición get, recibiremos un objeto req y respondemos un objeto res
  res.end('Hello World!!!');
})

app.listen(3000, () => {
  console.log('Servidor funcionando!');
});

Ahora agregaremos el comando start en los scripts del package.json, que deberá quedar así:

{
  "name": "primer-server-en-express",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start":"node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  }
}
**Importante que cada linea se separa con una coma!!!!**

Express lo que hace es hacer todo más simple y permite separar las funciones.....es más mantenible y modular.

Middleware Stack

**Routing en express**

Dependiendo de lo que usuario pida, el servidor lo tiene que manejar. 

Lo hace a través de rutas http: get, post, put(Actualizarlo), delete... 

Patrones: Regex (Expresiones regulares.)

Para poder acceder a otras rutas dentro del servidor, como http://localhost:3000/login debemos escribir:

app.get('/login',(req,res) => { // cuando recibamos una petición get, recibiremos un objeto req y respondemos un objeto res
  res.end('Aquí va el login!');
})

De lo contrario nos aparecerá 'Cannot GET /login'

Para poner algo en todas las rutas hacemos:

**Middlewares en express**

Son funciones para manejar peticiones que se activan por las rutas del express, del servidor....
Se ejecutan en el orden en que fueron agregadas. 

Mucha gente ha creado middlewares para express, solo deben ser instalados para usarse....

Middlewares tratan rutas u objetos que reciben del navegador. 

**Usando el módulo Morgan**

Es un registrador de peticiones de http.

Vamos a la consola y ponemos: npm install morgan.

Completamos el requerimiento en index.js así:

const morgan = require('morgan');

Se escribe: 
app.use(morgan('dev'));

Que imprime en consola algo como esto: 
GET / 200 1.117 ms - -

Claramente es un registrador de peticiones. 

Los middlewares de express se pueden buscar como _express middlewares_ en npmjs.org

**Configuraciones o settings para el servidor express**

Delimitamos con comentario la sesión settings....

Funciones para ** agregar variables**

Se crea una variable appName con 'Mi primer servidor' utilizando la función app.set, son funciones de express.

app.set('appName','Mi primer server');

y al final, en la misma función que imprime: 'Servidor funcionando' en consola, le decimos:

console.log('Nombre de la app',app.get('appName')); //Imprima el 'Nombre de la app' y concaténelo con el valor de appName. 

**Templates o plantillas**

HTMLS que se pueden re utilizar. 

Para reutilizar estás configuraciones en HTML utilizamos motores de plantillas como EJS, ERB, JInja2, Razor, Jade, Pug....

Más conocidos: **EJS, Jade, Pug**

EJS es como HTML pero con bucles y condicionales, etc... Le agregan funcionalidad de lenguaje de programación a nuestro HTML. 
Express no tiene ningún motor de plantilla pre instalado así que uno puede instalar el que quiera. 

Para configurar el motor de plantilla vamos a utilizar la función app.set

app.set('view engine','ejs');

Dentro de nuestra carpeta, creamos una nueva carpeta llamada views.

**__dirname --> muestra la dirección del archivo actual**

app.set('views',__dirname + '/views')Es la dirección de la carpeta views asignada a la variable 'views'__

nos queda:

app.set('views',__dirname + '/views');__

Esto amplia las carpetas de donde puedo escojer.... 

Ahora creamos un **index.ejs dentro de la carpeta views**

Este va a ser un documento con el formato de HTML, puede verse así:

<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title>MI PRIMER SERVER</title>
  </head>
  <body>
    <h1>HOLA CON EXPRESS Y EJS</h1>
  </body>
</html>

Para mostrar esto volvemos a nuestro documento index.js y vamos a la función que responde a las peticiones: app.get(/) (cuando recibe un get en el home).

Debería quedar así.

**node y express no sabes nada de ejs así que toca instalarlo con el siguiente comando:**
 
npm install ejs

Debería aparecer en el json.

**EJS es uno de los POCOS MÓDULOS QUE NO TOCA REQUERIR**

¿Por qué ejs y no HTML? Por lo visto express permite hacer cosas con HTML.... EJS permite la reutilizacion de código....

Vamos a crearle una navegación a los views así:

Creamos una carpeta en views ue se llamará partials. A partials le vamos a crear una nacegación, un archivo ejs llamado navigation.ejs

En ese archivo solo escribiremos navegación de inicio y de login:

<nav>
  <ul>
    <li>Inicio</li>
    <li>Login</li>
  </ul>
</nav>

Esto lo vamos a insetar en nuestro index de ejs utilizando **notación de ejs**

<%- include('partials/navigation.ejs') %>

Le agregamos los links a la navegación y queda así:

<nav>
  <ul>
    <li><a href="/">Inicio</a></li>
    <li><a href="/login">Login</a></li>
  </ul>
</nav>

Pero en la página de login no hay forma de devolverse, así que cambiamos la función que responde a login. app.get() de login.

Queda así:
app.get('/login',(req,res) => { // cuando recibamos una petición get, recibiremos un objeto req y respondemos un objeto res
  res.render('login.ejs');
})

Creamos el archivo dentro de views: login.ejs

En login.ejs creamos un formulario, y un h1 que diga login debería quedar así:

**Un API puede tener sub APIs**

Vamos a subdividir las aplicaciones a través de un módulo llamado router o enrutador.

Vamos a cortar (en este caso comentar) todas las rutas.... de index.js y pegarlo en un nuevo documento routes.js que se encuentra en la carpeta principal junto a index.js

Para comunicarnos con el otro archivo vamos a requerir express.

Vamos a requerir una función de express llamada Router.

Cambiamos el app, que es la función que inicializaba el servidor, por Router. 

Router es un objeto que estamos exportando así que debemos poner module.exports = router; al final

Debería quedar así:

const express = require('express');
const router = express.Router();

router.get('/',(req,res) => { // cuando recibamos una petición get, recibiremos un objeto req y respondemos un objeto res
  res.render('index.ejs');
})

router.get('/login',(req,res) => { // cuando recibamos una petición get, recibiremos un objeto req y respondemos un objeto res
  res.render('login.ejs');
})

router.get('*',(req,res) => { // cuando recibamos una petición get, recibiremos un objeto req y respondemos un objeto res
  res.end('Archivo no encontrado');
})

module.exports = router;

Para enlazarlo con index.js escribimos:

require('./routes.js') Requiere el archivo routes que está en la dirección en la que está este archivo. 

Creamos una constante que son las rutas: 

const routes = require('./routes.js')

y utilizamos la función abajo asi:

app.use(routes);

const express= require('express');
const morgan = require('morgan');
const app= express();
//Requiriendo rutas

const routes = require('./routes.js')

//Settings
app.set('appName','Mi primer server');
app.set('views',__dirname + '/views'); // ampliando las carpetas que puedo coger....
app.set('view engine','ejs');


//Middlewares
//Use es un middleware
app.use(morgan('dev'));
/* app.use(function (req, res, next){
  console.log('request url:' + req.url); //Imprima request url: y el url que se le está pidiendo al servidor a través de una peticiónti
  next();
});*/

/* app.use((req,res,next) => {
  console.log('ha pasado por esta funcion');
  next();
} );

//Router

app.use(routes);

app.listen(3000, () => {
  console.log('Servidor funcionando!');
  console.log('Nombre de la app: ',app.get('appName')); //Imprima el 'Nombre de la app' y concaténelo con el valor de appName.
});

**Esto mismo se puede hacer con un API**

Creamos un archivo llamado routes-api.js en la carpeta principal

Agregamos express, agregamos la constante de Router....

const express = require('express');
const router = express.Router();

Hacemos una función para que cuando le pidan '/' (La ruta inicial a esta ruta) responda un json (Siempre usando module.exports = router;) así:

const express = require('express');
const router = express.Router();

router.get('/', (req,res) => {
res.json({
  miprimerApi: 'Work!'
});

});

module.exports = router;

Para conectar con indes.js agregamos una constate para este router.... Quedarían las constantes así:

const routes = require('./routes.js');
const routesApi = require('./routes-api.js');

Las rutas quedarían así:

//Requiriendo rutas

const routes = require('./routes.js');  (Ya la teníamos, era la del router anterior)
const routesApi = require('./routesApi.js'); (La que acabamos de agregar)

Y usamos el app.use(routesApi)  

En las rutas abajo....

app.use('/api',routesApi); //Cuando recibamos una petición get /api use routes API

Las rutas deberían quedar así:

//Router

app.use(routes);
app.use('/api',routesApi); //Cuando recibamos una petición get /api use routes API
app.get('*',(req,res) => { // cuando recibamos una petición get, recibiremos un objeto req y respondemos un objeto res
  res.end('Archivo no encontrado');
}) 






















 
 



