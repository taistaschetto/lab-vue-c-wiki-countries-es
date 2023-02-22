![logo_ironhack_blue 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# LAB | Vue.js WikiCountries (Composition API)

## Introducción

Después de pasar demasiado tiempo en GitHub, encontraste un conjunto de [datos JSON de países](https://ih-countries-api.herokuapp.com/countries) y decidiste usarlo para crear ¡tu Wikipedia de países!

<p align="center">
  <img src="https://education-team-2020.s3.eu-west-1.amazonaws.com/web-dev/labs/lab-wiki-countries-1.gif" alt="Example - Finished LAB" />
</p>

## Setup

- Haz un fork de este repo
- Clona este repositorio
- Abre el LAB y comienza:

  ```bash
  $ cd lab-vue-c-wiki-countries-es
  $ npm install
  $ npm run dev
  ```

## La presentación

Al terminar, ejecuta los siguientes comandos:

```shell
$ git add .
$ git commit -m "done"
$ git push origin master
```

Cree una solicitud de extracción para que sus tutores puedan comprobar su trabajo.

## Empezando

Limpia el componente `App.vue` para que tenga la siguiente estructura dentro de las etiquetas de plantilla

```vue
<!-- src/App.js -->
<template>
  <div class="app"></div>
</template>
```

<br>

## Instrucciones

### Iteración 0 | Instalación de vue Router

Recuerda instalar vue Router:

```shell
$ yarn install vue-router
```

o

```bash
npm install vue-router
```
Y configurar el router en tu archivo `src/router/index.js`:

```js
// src/router/index.js
import { createRouter, createWebHistory } from "vue-router";

const routes = [
  {
    path: "/",
    name: "list",
    component: () => import("../components/CountriesList.vue"),
    children: [
      {
        path: "/list/:alpha3Code",
        name: "list",
        component: () => import("../components/CountryDetails.vue"),
      },
    ],
  },
];

const router = createRouter({
  history: createWebHistory("/"),
  routes,
  scrollBehavior() {
    document.getElementById("app").scrollIntoView();
  },
});

export default router;
```
Luego úsalo en el archivo `main.js` de la siguiente manera:

```jsx
// src/main.js
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";

createApp(App).use(router).mount("#app");
```

### Instalación de Bootstrap

Usaremos [Bootstrap](https://getbootstrap.com/) para el diseño :+1:

```shell
$ yarn install bootstrap
```

o

```bash
npm install bootstrap@v5.2.2
```

Para hacer que los estilos de Bootstrap estén disponibles en toda la aplicación, importa la hoja de estilos en `main.js` antes de la línea `createApp`:

```javascript
// src/main.js

import "bootstrap/dist/css/bootstrap.css";
```

### Iteración 1.1 | Crear componentes

En esta iteración, nos centraremos en el diseño general. Antes de comenzar, dentro de la carpeta src, crea la carpeta `components`. Allí crearás al menos 3 componentes:

- `Navbar`: Muestra la barra de navegación básica con el nombre del LAB.
- `CountriesList`: Muestra la lista de enlaces con los nombres de los países. Cada enlace debería ser un `router-link` de `vue-router-dom` que usaremos para <u>enviar</u> el código de país (`alpha3Code`) a través de la URL.
- `CountryDetails`: Este es el componente que renderizaremos a través de la `Route` de `vue-router` y <u>recibirá</u> el código de país (`alpha3Code`) a través de la URL.
  Este es en realidad el identificador del país (por ejemplo: `/ESP` para España, `/FRA` para Francia).

Para ayudarte con la estructura de los componentes, te hemos dado un ejemplo de página dentro de `example.html`.

Si quieres darle estilo, refresca tu memoria en Bootstrap en la [documentación](https://getbootstrap.com/docs/5.2/getting-started/introduction/) o mira cómo abordamos el estilo en `example.html`.

### Iteración 1.2 | Componente Navbar

La forma más sencilla de definir un componente en Vue es escribir una función de JavaScript, también conocida como función de componente. El Navbar debe mostrar el título *LAB - WikiCountries*.

### Iteración 1.3 | Componente CountriesList

Este componente debe renderizar una lista de `router-link`s que se utilizan para activar el cambio de URL del navegador. Al hacer clic en un componente `router-link`, se activará la `Route` correspondiente mostrando el componente de detalles del país.

### Iteración 1.4 | Componente CountryDetails y configuración de `router-view`

Ahora que nuestra lista de países está lista, debemos crear la página `CountryDetails`. `CountryDetails` muestra los detalles del país según el enlace en el que hicimos clic. Este componente debe mostrarse/renderizarse dinámicamente con `<router-vue />`.

```jsx
<!-- Example -->

<router-view>
```
Los componentes renderizados con Vue.js pueden leer la consulta con `this.$route`. Podemos usar esto para obtener la información que proviene de la barra de URL del navegador, por ejemplo, el código `alpha3Code` del país.

**NOTA:** Para la pequeña imagen de la bandera, puedes usar el código `alpha2Code` en minúsculas e incrustarlo en la URL como se muestra a continuación:

- Francia: https://flagpedia.net/data/flags/icon/72x54/fr.png
- Alemania: https://flagpedia.net/data/flags/icon/72x54/de.png
- Brasil: https://flagpedia.net/data/flags/icon/72x54/br.png
- etc.

---

### Iteración 2 | Vinculando todo juntos

Una vez que haya terminado de crear los componentes, la estructura de elementos que se renderizará en su `App.vue` debería parecerse algo así:

```vue
<template>
  <div class="app">
    <NavBar />
    <router-view />
  </div>
</template>

<script setup>
import NavBar from "./components/NavBar.vue";
</script>

<style></style>
```

---

### Iteración 3 | Establecer el estado cuando el componente se monta

Nuestra aplicación `App.vue` debe cargar `countries` en el método de datos de Vue, manteniendo los datos que provienen del archivo `src/countries.json`.

---

### Iteración 4 | Bonus | Obtener datos de países desde una API

En lugar de depender de los datos estáticos que vienen de un archivo `json`, hagamos algo más interesante y obtengamos los datos desde una API real.

Hagamos una solicitud `GET` a la URL [https://ih-countries-api.herokuapp.com/countries](https://ih-countries-api.herokuapp.com/countries) y usemos los datos devueltos de la respuesta como la lista de países. Puedes usar tanto `fetch` como `axios` para hacer la solicitud.

Si estás utilizando la API de opciones, debes usar el gancho `mounted()` para establecer el gancho de ciclo de vida que se ejecuta solo una vez y hace una solicitud a la API.

Debes usar `<script setup>` para establecer el método de ciclo de vida de la API de composición que te permite trabajar al comienzo del ciclo de vida.

<br>

<p align="center">
  <img src="https://vuejs.org/assets/lifecycle.16e4c08e.png" alt="Where is the setup lifecycle" />
</p>

<br>

La solicitud debe ocurrir lo primero cuando se carga la aplicación, por lo tanto, piensa en cuándo y desde dónde debemos hacer la solicitud a la API.

---

### Iteración 5 | Bono | Obtener los datos de un país desde una API

Crea el componente `CountriesDetails`. Debería hacer una solicitud a la API de RestCountries y obtener los datos para un país específico. Puedes construir el punto final de la solicitud utilizando el `alpha3Code` del país. Ejemplo:

- Estados Unidos: [https://ih-countries-api.herokuapp.com/countries/USA](https://ih-countries-api.herokuapp.com/countries/USA)
- Japón: [https://ih-countries-api.herokuapp.com/countries/JPN](https://ih-countries-api.herokuapp.com/countries/JPN)
- India: [https://ih-countries-api.herokuapp.com/countries/IND](https://ih-countries-api.herokuapp.com/countries/IND)

El efecto debería ejecutarse después de la renderización inicial y cada vez que cambie el parámetro de URL con el `alpha3Code`.

<br>

Happy coding! :heart:
