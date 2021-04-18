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


Abajo, usamos la función pool para caracterizar el acceso a postgres así: (se exporta como una constante)

export const pool = new Pool ({
user: 'santiagojeda',
host: 'localhost',
password: '271293So',
database: 'typescriptdatabase',
port: 5432,
});

5432 es el puerto por default, se consulta haciendo

\conninfo

En la consola de postgres

Este será un objeto que permite hacer consultas en la base de datos.

Ya puedo hacer consultas. Ahora voy a index.ts para hacer unas cuantas rutas, urls para el servidor. 

En src crearé una carpeta llamada routes y dentro de routes un index.ts también, pero en otra locación. 

En este index.ts importamos el router desde express:

import {Router} from 'express'

ejecuto la función, que nos devuelve un objeto que va a ser guardado en una constante:

const router = Router();

Y exportamos para que pueda ser utilizado desde otros archivos:

Defino qué pasa cuando mandan una petición get a la dirección /test:

router.get('/test', (req,res) => res.send('Hello World'))

Ahora vamos a probar el router, vamos a index.ts del src e importamos desde la carpeta routes el archivo index, así:

import indexRoutes from './routes/index'

Y para utilizarlo:

app.use(indexRoutes);

Ahora volvemos a correr: 

npm run dev

Ahora ya tenemos el enrutador

Antes de las rutas, debo ejecutar unos cuantos middlewares que me permitan, por ejemplo, leer archivos json:

Por lo tanto, antes de las rutas, ejecuto:

//Middlewares

app.use(express.json()); // para convertir las cosas a json (Desde Rest API)

app.use(express.urlencoded({extended: false})); //si le mandamos datos desde un formuario, puede convertirlos a json. (HTML)

Queremos utilizar estas confguraciones en las rutas.

Vamos a ir a index de routes para quitar el ''hello world'' y definir las rutas.

Vamos a definir el res.send en un archivo por aparte. Entonces vamos a cambiar lo del helloworld por:


Dentro de src hago una carpeta que se llama controllers y dentro de controllers hago un archivo que se llama index.controllers

En el nuevo archivo escribo:

import {Request,Response} from 'express'   // importando de express las interfaces de request y response

export const getUser = (req: Request,res: Response) =>{
    res.send('users')
}

Y ahora desde index de routes voy a importar los controladores

Pongo:

import {getUser} from '../controllers/index.controller'  // carpeta está subiendo un nivel porque estoy en routes.

Guardamos para que se reinicie el servidor y ahora al poner en el explorador: http://localhost:4000/users vemos la palabra users.

En index de routes vamos a copiar el: router.get('/users', getUsers); varias veces

router.get('/users/:id', getUsers); //Para cuando queramos un id específico....

router.post('/users', getUsers); //Para agregar usuarios....

router.put('/users', getUsers); //Para actualizar

router.delete('/users/id:', getUsers); //Para eliminar un usuario

Ahora, cómo conectar con la base de datos? 

Voy a index.controllers e importo todas las instrucciones de conexión a la base de datos....

Ponemos:
import {pool} from '../database'

export const getUsers = async (req: Request,res: Response) =>{
    await pool.query('SELECT * FROM users'); //Esto es una petición asíncrona (por esto el await) para mostrar el contenido como antes en "Consola" pero ahora como un arreglo....
    res.send('users')
}

Finalmente Queda: (Agregando Query Result tras importarlo)

import {Request,Response} from 'express'
import {QueryResult} from 'pg'


import {pool} from '../database'

export const getUsers = async (req: Request,res: Response) =>{
    const response: QueryResult = await pool.query('SELECT * FROM users'); 
    res.send('users')
}

Al hacer la petición a http://localhost:4000/users

Veremos en consola el arreglo con dos objetos


Ya no quiero ver eso por consola, quiero verlo en el explorador al hacer la petición. Así que voy a cambiar las cosas un poco:

Para que eso salga en el explorador entonces hago...

export const getUsers = async (req: Request,res: Response) =>{
    const response: QueryResult = await pool.query('SELECT * FROM users'); 
    res.status(200).json(response.rows);
}

Vuelvo a hacer la petición a: http://localhost:4000/users y veo el arreglo con dos objetos.

Para hacer más descriptivo el código anterior, trabajaremos con promise y return así:

export const getUsers = async (req: Request,res: Response): Promise <Response> =>{ // Esta función retorna una respuesta basada en una promesa, es decir que la retorna una vez acabe una petición asíncrona....
    const response: QueryResult = await pool.query('SELECT * FROM users'); 
    return res.status(200).json(response.rows);
}
Ponemos todo en un try catch así:
 
 export const getUsers = async (req: Request,res: Response): Promise <Response> =>{
    try{
        const response: QueryResult = await pool.query('SELECT * FROM users');
        return res.status(200).json(response.rows); 
    }
    catch(e) {
        console.log(e)
        return res.status(500).json('Internal Server Error')
    }
    
}
 
 
Seguimos agregando funciones, por ejemplo en Get users by id: (Debo actualizar todo también en el index de router para que importe y use las nuevas funciones)

export const getUserbyId = (req:Request, res:Response): Promise<Response> => {
console.log(req.params.id)  //req.params.id es el id que yo le ponga en el url
res.send('received')
}

Ahora, para mostrarlo en el explorador al hacer la petición:

export const getUserbyId = async (req:Request, res:Response): Promise<Response> => {
const id = parseInt(req.params.id)
const response: QueryResult = await pool.query('SELECT * FROM users WHERE id= $1', [id]);
return res.json(response.rows);
}
 
Siguiendo con la función de crear usuario.... comenzamos escribiendo en index.controllers:

export const createUser = async (req:Request, res:Response): Promise<Response> => {
   console.log(req.body) 
    res.send('received')
}
 
pero el req.body es la info que va a estar enviado las aplicaciones cliente. Para hacer un request necesitaríamos una aplicación movil, una aplicaciónde javascript o un formulario html, obviamente eso lleva tiempo entonces.
 
Para probar el rest api bajamos imsomnia buscando en google: insomnia rest

En insomnia creamos una petición llamada Typescript postgres y escribimos:

http://localhost:4000/users

Debería mostrar el contenido de la tabla....

Ahora intentamos hacer una peticion post, que es la del createUser:

Ahí podemos enviarle datos, por ejemplo un json así:

{
	"hello": "hello world"
}

Deberia devolver receeived ....

Y por consola mostrar el objeto que le he enviado.

Al ver que sí funciona vamos a cambiar un poco createUser

Hago esto:

export const createUser = async (req:Request, res:Response): Promise<Response> => {
   const{name, email} = req.body; //guardo en cosntante el nombre y el email del body que recibo....
   console.log(name,email)
    res.send('received')
}
Al enviarle,  por ejemplo, un json: 
 {
	"name": "John",
	"email":"john@gmail.com"
}
 
Debería poner received y recibir por consola.
 
export const createUser = async (req:Request, res:Response): Promise<Response> => {
   const{name, email} = req.body;
   pool.query('INSERT INTO users (name, email) VALUES($1,$2)',[name,email]) //inserto el primer y segundo parámetro que son name y email
    res.send('received')
}
 
Ahora al probarlo desde insomnia debería responder:

{
  "message": "User created successfully",
  "body": {
    "user": {
      "name": "John",
      "email": "john@gmail.com"
    }
  }
}

Deberíamos poder ver en postgres que se creó un nuevo usuario usando el comando: select * from users;

Ahora para actualizar y eliminar....

Comenzaremos por Eliminar, que es muy sencillo, solamente debemos confirmar si recibimos el id...

Descomentamos el delete user tanto en controllers como en index de routes, lo importamos en routes y comenzamos a escrivir dentro de la función en controllers:

export const deleteUser = (req:Request, res:Response): Promise<Response> => {
    console.log(req.params.id)
    res.send('Deleting');
} 
	
Si recibe el id devuelve deleting...

Al mandar la petición delete a /users/1 debería mostrar deleting y aparece 1 en consola....

export const deleteUser = async (req:Request, res:Response): Promise<Response> => {
    const id = parseInt(req.params.id);
    await pool.query('DELETE FROM users WHERE id= $1', [id]); //no tengo que gurdarla en constante
    return res.json(`User ${id} deleted successfully`); //Concatenación javascript
} 
	
Debería verse en consola y hacer el get users que se eliminó el usuario 1

Ahora, por útimo, para actualizar.... Descomentamos en routes y en la importada... updateUser

Quedan así:
import {getUserbyId, getUsers, createUser,deleteUser,updateUser} from '../controllers/index.controller'

router.get('/users', getUsers);
router.get('/users/:id', getUserbyId); 
router.post('/users', createUser);
router.put('/users/:id', updateUser);
router.delete('/users/:id', deleteUser);

Escribo la función así, necesitando 3 cosas.... id, name y email (Estos dos los saco de body de la petición)

export const updateUser = async (req:Request, res:Response): Promise<Response> => {
    const id =parseInt(req.params.id);
    const{name, email} = req.body;
    await pool.query('UPDATE users SET name $1, email $2 WHERE id =$3',[name, email,id]);

    return res.json(`User ${id} updated successfully`)
}

Necesito el req.params.id para saber cuál quiero actualizar y el req.body los nuevos datos para actualizar.

Hago la petición en la ruta que quiero actualizar...

Ahora para convertir el código simplemente cierro todas las ventanas, cancelo la consola y pongo:

npm run build

Podría poner:

node dist/index.js

Y activa el servidor para seguir consultando









 
 


















          

6. 
7. 




