# NextJS
Documentacion guia https://nextjs.org/learn/dashboard-app/optimizing-fonts-images

### Styling [Docs](https://nextjs.org/learn/dashboard-app/css-styling)
> Existen varias formas de manejar los estilos en nextjs:
- Tailwind, framework de css.
- Css Modules [Docs](https://nextjs.org/docs/app/getting-started/css#css-modules), le permite manejar alcance de los estilos mediante el uso de nombre de clases unicas, para que no haya problemas de colision de estilos.
- Clsx, le permite condicionar estilos basado en el estado de la aplicacion.

### Optimizacion de Fonts e imagenes [Docs](https://nextjs.org/learn/dashboard-app/optimizing-fonts-images)
> NextJS automaticamente optimiza los fonts de la aplicacion cuanto utiliza el modulo `next/font`. Descarga el archivo de fonts al tiempo de compilacion y los hostea con los assets estaticos junto con la aplicacion.

> La optimizacion de imagenes en NextJS se hace con el uso del modulo `next/image`, este modulo tiene el componente `<Image>` que viene con optimizaciones de manera automatica como:
- Previene el movimiento del contenido cuando las imagenes estan cargando.
- Ajusta el tamaño de manera automatica para evitar servir imagenes grandes a puertos de vista pequeños.
- Carga las imagenes con `lazy load` por defecto (las imagenes cargan cuando se entra al `view port`).
- Permite servir las imagenes en formatos modernos como `webp` y `avif`, cuando el buscador lo soporta.
Consumir imagenes externas de manera segura [Docs](https://nextjs.org/docs/app/getting-started/images#remote-images).
Optimizacion de fonts [Docs](https://nextjs.org/docs/app/getting-started/fonts).
Como google maneja JavaScript a traves del proceso de indexacion [Docs](https://vercel.com/blog/how-google-handles-javascript-throughout-the-indexing-process).

### Layouts y Pages [Docs](https://nextjs.org/learn/dashboard-app/creating-layouts-and-pages)
> Los archivo `Page` son un tipo de archivo especial de NextJS que le indica que un comoponente quiere ser accesible desde una ruta, usted puede crear rutas nesteadas dentro de la carpeta `app/` y subcarpetas de esta misma [Nested routing](https://nextjs.org/learn/dashboard-app/creating-layouts-and-pages#nested-routing).

> El archivo `Layout` es un tipo de archivo especial de NextJS que se usa UI que es compartida entre varias paginas `Pages`.
Cuando se usan `Layouts` la navegacion en la pagina solo renderizara los componentes `page` mientras los `Layouts` no seran renderizados nuevamente, esto es llamado [renderizado parcial](https://nextjs.org/docs/app/getting-started/linking-and-navigating#4-partial-rendering), el cual preserva el estado del lado del cliente de React en el `Layout` cuando se transiciona entre paginas.

> El arhivo `Layout` que se encuntra en la raiz del proyecto sobre `app/layout` se llama root layout y es requerido en toda aplicacion que use NextJS, cualquier elemento de UI que agregue al root layout sera compartido por todas las paginas `page` de toda la aplicacion. Puede utilizar su root layout para modificar los tags `<html>` y `<body>`, tambien puede añadir metadata desde este layout.

> Cada ruta anidada puede tener su propio `layout` para compartir UI solo entre sus componentes sin afectar las demas rutas.

### Navegacion entre paginas [Docs](https://nextjs.org/learn/dashboard-app/navigating-between-pages)
> Generalmente para navegar entre paginas se utiliza el tag `<a>` cuando se utiliza este tag toda la pagina se recarga, NextJS para evitar hacer esto tiene el comoponente `<Link>` precarga el codigo de las rutas linkeadas para que cuando el usuario haga `click` en un componente el cambio sea instantaneo. **Como mostrar links activos usando clsx [Docs](https://nextjs.org/learn/dashboard-app/navigating-between-pages#pattern-showing-active-links)**.

### Conectando a una base de datos
NextJS [Docs](https://nextjs.org/learn/dashboard-app/setting-up-your-database)
Prisma [Docs](https://www.prisma.io/docs/guides/nextjs)

### Fetching data [Docs](https://nextjs.org/learn/dashboard-app/fetching-data)
#### Componentes de servidor para traer data
> Por defecto NextJS utiliza comoponentes a nivel de servidor esto tiene los siguientes beneficios:
- Los componentes a nivel de servidor soportar promesas de JavaScript permitiendo traer data de manera asincrona `async/await`.
- Componentes a nivel de servidor corren del lado del "servidor", con esto puede mantener las consultas de datos mas costosas a nivel de servidor y entregar el resultado al cliente.
- Los componentes a nivel de servidor al ejecutarse a nivel del "servidor" permiten que la conectividad a otras APIs se haga de manera segura sin exponer la peticion.
- Los componentes a nivel de servidor no necesitan de ninguna capa de API adicional como [la que se presenta aca](https://nextjs.org/learn/dashboard-app/fetching-data#api-layer), ya que las peticiones se hacen directamente a los servicios.

#### Peticiones en paralelo y cascada [Docs](https://nextjs.org/learn/dashboard-app/fetching-data#parallel-data-fetching)
> Las peticiones en cascada son peticiones donde para poder ejecutar la siguiente peticion primero debe terminar la actual, generalmente esto no esta mal pero si quiere utilizar peticiones paralelas tambien lo puede hacer con [Promise.all()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) y [Promise.allSettled()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled) esto le permite ejecutar peticiones de manera paralela sin esperar que una en especifico termine, el unico problema de este metodo es que pasa si una de las peticiones es mas lentas que las demas?, esto termina afectando a toda la peticion.

ej:
```JavaScript
export async function fetchCardData() {
  try {
    const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
    const customerCountPromise = sql`SELECT COUNT(*) FROM customers`;
    const invoiceStatusPromise = sql`SELECT
         SUM(CASE WHEN status = 'paid' THEN amount ELSE 0 END) AS "paid",
         SUM(CASE WHEN status = 'pending' THEN amount ELSE 0 END) AS "pending"
         FROM invoices`;
 
    const data = await Promise.all([
      invoiceCountPromise,
      customerCountPromise,
      invoiceStatusPromise,
    ]);
    // ...
  }
}
```

### Renderizado estatico y dinamico [Docs](https://nextjs.org/learn/dashboard-app/static-and-dynamic-rendering)
#### Renderizado estatico
> Con el renderizado estatico las consultas de datos se hacen al momento de construir la aplicacion `build time` o cuando se revalida la data, cuando un usuario visita la pagina el contenido que se le muestra es el contido que ha sido cachado con el renderizado estatico previamente, con esto puede:
- Tener sitios mas rapidos, ya que la data es prerenderizada en el momento de compilacion.
- Bajar la carga a los servidores, ya que el servidor no necesita generar contenido dinamicamente.
- Mejoras de SEO por la prerenderizacion del contenido que es mas facil de indexar por los crawlers de los motores de busqueda.
> Este tipo de renderizado es util cuando su data no cambia mucho o es data que se comparte entre muchos usuarios.

#### Renderizado dinamico
> Con el renderizado dinamico el contenido es renderizado cada vez que se hace una peticion, con esto puede:
- Tener data en tiempo real, ideal para aplicaciones donde la data cambia todo el tiempo.
- Contenido especifico por usuario, es mas facil servir contenido personalizado como dashboards, perfiles o actualizar informacion de usuarios o servicios de este modo.
- Informacion que solo es conocida al momento de la peticion como las cookies o parametros de busqueda en la URL.
> Problemas con el renderizado [dinamico](https://nextjs.org/learn/dashboard-app/static-and-dynamic-rendering#simulating-a-slow-data-fetch), con el renderizado dinamico su aplicacion va a ser tan rapida como su peticion mas lenta.

### Streaming [Docs](https://nextjs.org/learn/dashboard-app/streaming)
