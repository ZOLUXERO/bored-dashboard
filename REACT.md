# React core concepts
### Components [Docs](https://nextjs.org/learn/react-foundations/building-ui-with-components)
> Los componentes le permite crear los bloques de construccion de su aplicacion, permitiendo que su codigo sea modular y se pueda reutilizar a traves de una aplicacion cuando lo necesite, le permite actulizar, agregar y elminar sin necesidad de tocar toda la aplicacion.

Ej:
```JavaScript
function Header() {
  return <h1>Develop. Preview. Ship.</h1>;
}
<Header />
```

### Props [Docs](https://nextjs.org/learn/react-foundations/displaying-data-with-props)
> Los props son argumentos personalizados que le puede pasar a los componentes para conseguir diferentes comportamientos en un mismo componente.
> Estos props son objetos y se puede hacer uso de [object destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring), para llamar explicitamente los valores que ud necesita de un prop.

Ej sin **object destructuring**:
```JavaScript
function Header(props) {
  console.log(props); // { title: "React" }
  return <h1>Develop. Preview. Ship.</h1>;
}
<Header title="React" />
```
Ej con **object destructuring**:
```JavaScript
function Header({ title }) {
  return <h1>{title}</h1>;
}
<Header title="React" />
```

### State [Docs](https://nextjs.org/learn/react-foundations/updating-state)
> Si necesita que su sitio sea interactivo, lo puedo lograr con el `state` y los `event handlers`.
Los eventos usan camelCase ej: `onClick`, `onChange`, `onSubmit`.

Ej hacer que boton haga algo con eventos:
```JavaScript
function HomePage() {
  // 	...
  function handleClick() {
    console.log('increment like count');
  }
 
  return (
    <div>
      {/* ... */}
      <button onClick={handleClick}>Like</button>
    </div>
  );
}
```
#### State and Hooks
> Los hooks le pemrmiten agregar funcionalidad adicional a su aplicacion como el `state` o estado, el estado es la informacion que cambia con el tiempo en su aplicacion, generalmente se activa mediante la interaccion de un usuario.
Un ejemplo del estado seria guardar cuantas veces un usuario ha hecho `click` en el boton, el hook para manejar el estado en React es llamado `useState()`.

Ej:
```JavaScript
function HomePage() {
  // ...
  const [] = React.useState();
 
  // ...
}
```
> El hook `useState()` retorna un array [] los valores de este array se puede acceder usando object destructuring.

Puede agregar el primer item al array del estado el cual es el valor que puede declarar con cualquier nombre:
```JavaScript
function HomePage() {
  // ...
  const [likes] = React.useState();
 
  // ...
}
```

El segundo item en el array es una function que se utiliza para actualizar el valor del estado, siempre utiliza un prefix `set` seguido por el nombre de la variable que seteo `setLikes`, tambien puede darle un valor inicial al estado de `likes`(0):
```JavaScript
function HomePage() {
  // ...
  const [likes, setLikes] = React.useState(0);
 
  // ...
}
```

Finalmente puede utilizar el estado dentro de su componente y actualizarlo cuando el usuario haga click dentro de la funcion `handleClick()`:
```JavaScript
function HomePage() {
  // ...
  const [likes, setLikes] = React.useState(0);
 
  function handleClick() {
    setLikes(likes + 1);
  }
 
  return (
    <div>
      {/* ... */}
      <button onClick={handleClick}>Likes ({likes})</button>
    </div>
  );
}
```

El estado es iniciado y guardado dentro de un componente, puede pasarle la informacion del estado de un componente a otros componentes hijos por medio de `props` pero la logica para actualizar el estado deberia quedar en el componente donde se inicio el estado.

### Server and Client components [Docs](https://nextjs.org/learn/react-foundations/server-and-client-components)
Nextjs usa componentes de servidor por defecto.
