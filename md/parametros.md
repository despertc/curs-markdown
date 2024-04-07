
## Saltar a una ruta
Al hacer `.use(router)` en el fichero _main.js_ hacemos que esa variable (el router) esté disponible para todos los componentes desde `this.$router`. Esto nos permite acceder al router desde un componente para, por ejemplo, cambiar a una ruta.

Podemos cargar la ruta que queramos desde JS con
```javascript
this.$router.push(ruta)
```
Tenemos varios métodos para navegar por código:
* **`.push(newUrl)`**: salta a la ruta _newUrl_ y la añade al historial

* **`.replace(newURL)`**: salta a la nueva ruta pero reemplaza en el historial la ruta actual por esta
* **`.go(num)`**: permite saltar el num. de páginas indicadas adelante (ej. _this.$router.go(1)_) o atrás (_.go(-1)_) por el historial

## Paso de parámetros
Se pueden pasar parámetros a la ruta:

```javascript
this.$router.push({ name: 'users', params: { id: 123 }})
```

salta a la ruta con _name_ users y le pasa como parámetro una _id_ de valor 123 (si la ruta definida en _users_ fuera _/usuarios_ la URL cargada será `/usuarios/123`). En el componente que se cargue en dicha ruta obtendremos el parámetro pasado con `this.$route.params.nombreparam` (en el ejemplo en `this.$route.params.id` obtenemos el valor `123`). Podemos pasar cualquier objeto como parámetro.


También se puede pasar una _query_ a la ruta:
```javascript
this.$router.push({ path: '/register', query: { plan: 'private' }})
```

salta a la URL `/register?plan=private`. En el componente que se carga obtenemos la query pasada con `this.$route.query` (obtenemos el objeto, en el ejemplo `{ plan: 'private' }`).

## El objeto $route
Es un objeto que contiene información de la ruta actual (no confundir con _$router_). Algunas de sus propiedades son:
* **params**: el objeto con los parámetros pasados a la ruta (puede haber más de uno)
* **query**: si hubiera alguna consulta en la ruta (tras '?') se obtiene aquí un objeto con ellas
* **path**: la ruta pasada (sin servidor ni querys, por ejemplo de `http://localhost:3000/users?company=5` devolvería '/users')
* **fullPath**: la ruta pasada (con las querys, por ejemplo de `http://localhost:3000/users?company=5` devolvería '/users?company=5')

## Redireccionamiento. Not found
En una ruta podemos poner una redirección a otra en lugar de un componente. Es lo que haremos para que si se carga una ruta inexistente nos cargue un componente que le indique al usuario que la ruta no existe.

```javascript
  routes: [
    ...
    {
      path: '/not-found',
      name: '404',
      component: CompNotFound,
    },
    {
      path: "/:pathMatch(.*)*"
      redirect: {
        name: '404',
      },
    }
  ]
```
## Cambio de parámetros en una ruta
Si cambiamos a la misma ruta pero con distintos parámetros Vue reutiliza la instancia del componente y no vuelve a lanzar sus _hooks_ (created, mounted, ...). Esto hará que no se ejecute el código que tengamos allí. Por ejemplo supongamos que en una ruta '/edit/5' al cargar el componente se pide el registro 5 y se muestra en la página. Si a continuación cargamos la ruta '/edit/8' seguiremos viendo los datos del registro 5).

La forma de solucionar esto es usar el elemento 'beforeRouteUpdate' y realizar allí la carga de los datos:
```javascrip
beforeRouteUpdate (to, from, next) {
    // Código que responde al cambio. En 'to' tenemos la ruta anterior y en 'from' la nueva
    // antes de acabar hay que llamar a next()
    // Aquí cargamos los nuevos datos
    next();
} 
```

## Vistas con nombre y Subvistas
Podemos cargar más de un componente usando varias etiquetas `<router-view>`. Por ejemplo si nuestra página constará de 3  componentes (uno en la cabecera, otro el principal y otro en un _aside_ pondremos en el HTML:
```html
<router-view class="cabecera" name="top"></router-view>
<router-view class="main"></router-view>
<router-view class="aside" name="aside"></router-view>```
```


Para que se carguen los 3 componentes lo debemos indicar al definir las rutas:
```javascript
{
    path: '/',  
    components: {
        default: CompMain,		// CompMain se cargará en el <router-view> sin nombre
        top: CompCabecera,
        aside: CompAside
    }
}
```
Definiremos la ruta del siguiente modo:



```javascript
{
    path: '/',  
    components: {
{
  path: '/user/:id', 
  component: User,
  children: [
    { path: '', component: UserHome },
    { path: 'profile', component: UserProfile },
    { path: 'posts', component: UserPosts }
  ]
}
```
<div style="text-align: center">
<iframe width="860" height="515" src="https://www.youtube.com/embed/dEOJDiuiSww?si=3ohsOS7R8QdQlkIE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen>
</iframe>
</div>