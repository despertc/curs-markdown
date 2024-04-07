
## Instalación
Si al crear nuestro proyecto _Vue_ vamos a la opción _manual_ y seleccionamos el **Router** no es necesario hacer nada porque se configura todo automáticamente. 

### Añadir vue-router a un proyecto Vue3 ya creado
Vue3 sólo soporta versiones _vue-router_ superiores a la 4.x. Para asegurarnos que la instala ejecutaremos:
```bash
npm install -S vue-router@next
```
(o editaremos a mano la versión en el _package.json_)

El resto es todo igual excepto que en el fichero de rutas cambia las primeras líneas y el objeto que se exporta:


```javascript
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'
import About from '../views/About.vue'

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: About
  },
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

Y en el fichero _main.js_ se haría:
```javascript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router' // <---

createApp(App).use(router).mount('#app')
```

## Crear las rutas
Las rutas de nuestra aplicación las definiremos en el fichero **_router/index.js_**. Allí creamos la instancia para nuestras rutas (el objeto que exportamos) y la configuramos. También debemos importar todos los componentes que definamos en el router:


```javascript
import { createWebHistory, createRouter } from 'vue-router'

// Importamos los componentes que se carguen en alguna ruta
import AppHome from './components/AppHome.vue'
import AppAbout from './components/AppAbout.vue'
import UsersTable from './components/UsersTable.vue'
import UserNew from './components/UserNew.vue'
import UserEdit from './components/UserEdit.vue'

const routes = [
  {
    path: '/',
    name: 'home',
    component: AppHome
  },{
    path: '/about',
    name: about,
    component: AppAbout
  },{
    path: '/users',
    component: UsersTable
  },{
    path: '/new',
    component: UserNew
  },{
    path: '/edit/:id',
    component: UserEdit
    props: true
  }
];

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

El modo _'history'_ de nuestro router indica que use rutas "amigables" y que no incluyan la # (piensa que en realidad no se están cargando diferentes páginas sino partes de una única página ya que es una SPA). Esta es la opción que escogeremos siempre en las aplicaciones SPA, aunque si nuestro servidor web usa ASP.NET o JSP habrá que decirle que ignore las URLs porque ya se ocupa de ellas Vue. La alternativa sería usar `createWebHashHistory()` pero en ese caso las rutas en vez de ser algo como `http://localhost:8080/products` serían  `http://localhost:8080/#products`.

_VueRouter_ permite rutas dinámicas como la indicada para el componente _UserEdit_: la ruta `/edit/` seguida de algo más (ej. `/edit/5`) hará que se cargue el componente _UserEdit_ y que dicho componente reciba en un parámetro llamado `this.$route.params.id` la parte final de la ruta. Si añadimos a la ruta la opción `props: true` hacemos que el componente además reciba el parámetro en sus _props_ (en este caso recibirá una variable llamada _id_ con valor 5).

Para cada ruta que queramos mapear hay que definir:
* **path**: la url que hará que se cargue el componente
* **component**: el componente que se cargará donde se encuentre la etiqueta **\<router-view>** en el HTML

Además de esas propiedades podemos indicar:
* **name**: le damos a la ruta un nombre que luego podemos usar para referirnos a ella
* **props**: se usa en rutas dinámicas e indica que el componente recibirá el parámetro de la ruta en sus _props_. Si no se incluye esta opción el componente tendrá que acceder al parámetro _id_ desde `this.$route.params.id` 

Cada vez que pongamos una nueva URL en el navegador no cambiará todo el layout sino que sólo se cargará el componente indicado para esa ruta. En el fichero _App.vue_, en la parte del HTML en que queramos que se carguen los diferentes componentes de nuestra SPA incluiremos la etiqueta:
```html
<router-view></router-view>
```
!!!warning "Cuidado!"
    Cuando cambiemos la URL se cargará el componente indicado para la ruta actual.