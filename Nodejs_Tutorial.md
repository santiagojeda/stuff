
En este tutorial se crearán los primeros servidores.

Node.js permite ejecutar Javascript en el computador, javascript del lado del servidor. Sirve para hacer backend. 

**Instalar Nodejs en el computador**

Ir a nodejs.org y descargar el instalador de node.js para nuestro sistema operativo. 

Para comprobar que Node.js está instalado, se puede escribir en el terminal:

node -v o node --version

Deberá aparecer la versión de node instalada. 

Para editar código utilizaremos Visual Studio Code.

Se creará una carpeta llamada 'nodejs-curso' para guardar todo. Preferiblemente se crea la carpeta en el escritorio. Después, abrir Visual Studio Code y arrastrar carpeta. 

**Hola mundo de Javascript**

Utilizando Visual Studio code se crea un nuevo archivo llamado 'hola-mundo.js', esto creará un archivo de código de Javascript.
En la primera linea de este archivo de Javascript escribiremos:

console.log('Hola Mundo');

Guardamos los cambios. 

Para correr esta aplicación vamos a la terminal y escribimos:

cd Desktop

Esto nos ubicará en el Desktop.

Después:

cd nodejs-curso

Para ejecutar nuestro programa de hola-mundo.js  escribimos

node hola-mundo.js

Para crear una aplicación más grande necesitamos divirla en partes, estas partes en nodejs se concoen como módulos. Cada módulo tiene una tarea. 

Creamos un archivo 'index.js', suponiendo que vamos a crear una aplicación matemática. 

Dentro del archivo index vamos a crear 4 funciones para las operaciones matemáticas así:

function add(x1, x2) {
    return x1+ x2;

}

function substract(x1,x2) {
return x1- x2;
}

function multiply(x1,x2) {
    return x1 * x2;

}

function divide(x1,x2) {
if (x1==0){
    console.log("No se puede dividir por 0");
} else {
    return x1 /x2 ;
}
}
console.log(add(1,2));

En una aplicación real no ponemos toda las funciones en un solo archivo. Normalmente el Index solamente llama a las otras funciones.  Crearemos otro archivo llamado matematica.js o math.js en el que pegaremos las funciones matemáticas. 

Como se llaman a funciones que estan en otro archivo? Se 'importan'. Como ya no estamos trabajando en el navegador, en el que tocaría improtar tilizando el comando script src="" para cada archivo. En math.js vamos a utilizar **require('index.js')**

En el index entonces importamos las funciones a una constante asi:

const math = require('./math.js');
Debemos usar notacion punto para llamar la funcion:

No se usa:
console.log(divide(1,0)); sino: console.log(math.divide(1,0));

Para poder usar las funciones desde otro archivo, devo exportar las funciones utilizando **exports**. Por ejemplo:

exports.add = add; Queire decir: Cuando desde afuera llame add, utilice la funcion add. 

Se escribe entonces en el archivo math.js:

exports.add = add;
exports.substract= substract;
exports.multiply= multiply;
exports.divide= divide;

Al usar 
node index.js 

en la terminal, debería funcionar y arrojar los resultados de las operaciones así las funciones no estén en js.


Para comentar una linea: //

Para comentar varias lineas: /* para abrir y */ para cerrar /

Si se llama
console.log(math); en index, por ejemplo aparece:

{
  add: [Function: add],
  substract: [Function: substract],
  multiply: [Function: multiply],
  divide: [Function: divide]
}

Diciendo que al importar math se está importando un objeto con diferentes propiedades. 

Hay otra forma de crear un objeto:

En math, escribimos al principio:

const Math={}; Para crear el objeto.

Math.add = add;
Math.substract= substract;
Math.multiply= multiply;
Math.divide= divide;

Para agregar propiedades al objeto.

module.exports= Math;

Para exportar el objeto. 

entonces: exports.nombre= nvfjbvkf;  exporta una propiedad de un objeto.

module.export = nombre; exporta objetos, funciones variables y todo en javascript. 


Para hacer apicaciones web utilizaremos módulos pre construidos. 

Se importaría así, si existiera un módulo llamado math:

const math = require('math.js');

Para saber los módulos de nodejs: 

https://nodejs.org/en/docs/

Y se hace click en API, a la izquierda.
Ahí están todos los módulos de javacript. 

Para probar el modulo de OS se crea un archivo index2.js y se importa el modulo OS así:

const os = require('os');

Para probar una de las funciones de os, platform, utilizamos el siguiente comando.

console.log(os.platform());

Si es mac debería decir Darwin al correr el código en terminal. 

Hacemos los mismo para la función release.

console.log(os.release());

Para saber la memoria libr concatenamos un string y la función

console.log('Free mem:', os.freemem());

console.log('Total mem:', os.totalmem());

Aprenderemos a utilizar filesystem en un archivo index3.js Vamos a aprender a crear achivos nuevos y a poder leerlos.

const fs = require('fs');

Aprenderemos a crear archvos utilizando fs.writefile('./texto.txt'); ./ porque se crea en la misma carpeta en la que está index3.js, escribimos: (incluyendo un callback, que es una función que llamará al crear el archivo).

fs.writeFile('./texto.txt', 'linea uno', function(err){
    if (err){
        console.log(err);
    }
    console.log('Archivo creado');
    });
    
Indica si se pudo crear el archivo. 

Esto anterior es código asíncrono. Poder crear un archivo es tarea del sistema operativo, no de node.js, node.js invoca al sistema operativo. 

La versión 'bloqueante' de ese código sería

const result = fs.writeFile("","");

Esto implicaría que node.js ejecutaría fs.writeFile y cuando termine de ejecutarse vamos a recibir el resultado y seguir a las otras líneas de código. Al escribirlo de forma asincrónica, no necesita esperar a que termine de correr para seguir con las otras lineas de código. **Esto es lo que permite node.js**.

Node.js delega. 

Ahora vamos a utilizar fs.readFile(), le decimos el archivo que debe leer por parameyro y creamos una función que debe ejecutar al terminar. Esa función puede recibir los datos o recibir el error. Si recibe los datos muestra por consola (**deben ser convertidos a string utilizando toString()**) y si no, muestra el error. 

fs.readFile('./texto.txt',function (err,data){
    if(err){
        console.log(err);
    }
    console.log(data.toString());
});

Sin embargo la mayor función de node.js es crear servidores. 

Debemos usar http, vamos a aprender a utilizarlo en index4.js.

Las aplicaciones piden algo 'http' al servidor y el servidor responde algo. 

http: hypertext transfer protocol ----> Es el protocolo que se usa para la transferencia de peticiones. 

Importamos entonces http así:

const http=require('http');

Vamos a usar la función de crear servidor http.createServer

Le diremos que ejecute una función al terminar, una función así:

http.createServer(function(req,res){

});
Siendo req el request del cliente y res la respuesta del servidor. 

http.createServer(function(req,res){
    res.write('<h1>Hola Mundo desde Nodejs</h1>') // Te voy a responder con una línea de html
    res.end(); //Termina la respuesta para poder seguir haciendo peticiones.  
}).listen(3000); //Decirle en qué puerto va a escuchar mi servidor.


Al correr el código el cursor se queda esperando, así que debemos ir al explorador y poner: localhost:3000 que quiere decir que estamos escuchando un servidor que está en este mismo computador. Si se quiere iniciar el servidor de nuevo y ver cambios debe cancelarse la opción anterior y volverse a correr. 

Ahora vamos a incluir, dentro de create.Server() un head utilizando writeHead(), especificando el tipo de respuesta. 














