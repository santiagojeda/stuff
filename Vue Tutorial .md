**Vue Tutorial: **

Javascript Framework for eveloping frond-end applications easily



**Before Knowing Vue.us**

Es6 Module Syntax: Arrow functions.....

High Order Methods: forEach, map, filter...


When to use forEach?
.forEach() is great you need to execute a function for each individual element in an array. Good practice is that you should use .forEach() when you can’t use other array methods to accomplish your goal. I know this may sound vague, but .forEach() is a generic tool… only use it when you can’t use a more specialized tool.
When to use map?
.map() when you want to transform elements in an array.
When to use filter?
.filter() when you want to select a subset of multiple elements from an array.
When to use find?
.find() When you want to select a single element from an array.
When to use reduce?
.reduce() when you want derive a single value from multiple elements in an array.

Arrow functions....

Creating Todo App....

Reusable components.

Each component hold its own Data, hold its own methods.

**Anatomy of a Vue.js Component**

Output: HTML

Example:
<template>
<h1>{{user.name}}</h1> //user interpolation, to put dynamic content

Functionality: JS <script>
export.default {
  name: 'User',
  data() {
  return {
  user: {name:'Brad'}
  }
  }
  }

</script>
</template>
Style: 

<style>
  h1 {
  font-size: 2rem;
  }
</style>

Most professional Vue apps are going to use Vue CLi (Command Line interphase).

Features include Babel: (Compiles to JS), Typescript cna be used with Vue, EsLint, Post CS

Dev server with hot reload.

Includes Vue UI tool to manage the graph interphase of the app.

Vuex for application at bigger states....

1. Installing Vue CLI using npm in the terminal **NEED TO RUN WITH ADMIN (sudo)**
2. Adding Vue devtools extension to chrome.  
3. vue create project-name
Dentro de la carpeta project-name poner:

npm run serve

o desde el main directory: vue ui y comienza el ui, desde el que se puede crear un proyecto

Pasamos el proyecto a vscode

En index.html está lo que se muestra publicamente:

    <div id="app"></div>
    
Es un placeholder para nuestra aplicación, es parecido a react. 


En src/ main.js is basically the entry point of vue.js

Tiene vue importado, tiene App importado y renderiza el app component en un elemento que tenga el #app (El id de app).

El app.vue file tiene un template, un script y un style. Ese style va a ser global.

En HelloWorld.Vue hay un componente que podemos editar. 

















