**Tutorial de Directus**

Primero que todo, para comenzar un nuevo proyecto de directus hay que asegurarse de tener node 10+ instalado. Después de esto vamos a la terminal y:

npx create-directus-project example-project

En este caso:

npx create-directus-project arca-demo

Se especifica el tipo de database (En este caso postrgres), el local host, el puerto, el nombre de la database y la contraseña. Se crea entonces un usuario (Que debe ser un e-mail) Y una contraseña.

Después, ubicados en la carpeta que se creó, en este caso arca-demo, hacemos:

npx directus start y eso debería iniciar un servidor en el puerto 8055

