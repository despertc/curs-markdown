# UNIDAD 13 - SPA: Vue-router

En esta unidad vamos a ver todo lo relacionado con Vue-router

## Consejos

* Puedes seguir la tabla de contenidos mostrada en la barra lateral izquierda.
* Lee con detenimiento todo el contenido.
* Revisa el código y pruébalo.
* Pregunta al profesor en caso de dudas.

## Código de ejemplo

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
