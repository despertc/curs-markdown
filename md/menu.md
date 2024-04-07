## Crear un menú
Seguramente querremos un menú en nuestra SPA que nos permita ir a las diferentes rutas (que provocarán que se carguen los componentes). Para ello usaremos la etiqueta **\<router-link>**. Ejemplo:
```html
<router-link to="/">Home</router-link>
<router-link to="/about">Acerca de...</router-link>
```

!!!note "Nota"
    Cuando accedemos a una ruta su elemento \<router-link> adquiere la clase _.router-link-active_.


Si le hemos puesto la propiedad _name_ a una ruta podemos hacer un enlace a ella con:

```html
<router-link to="{name: 'nombre_de_la_ruta'}">Home</router-link>
```

Se puede hacer (aunque no es lo normal) una opción de menú a una ruta dinámica y pasarle el parámetro deseado. Por ejemplo para editar el usuario 5 haremos:
```html
<router-link to="{name: 'edit', params: {id: 5}}">Editar usuario 5</router-link>
```