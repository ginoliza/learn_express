+++
archetype = "chapter"
title = "3. Requests HTTP"
weight = 3
+++

## Tipos de Requests

- GET: cuando se quiere requerir un recurso

- POST : envía un recurso a el servidor, y este lo puede manipular como sea

- PUT: métodos actualizar, reemplazar un recurso con lo que se está enviando

- PATCH: se quiere parchar un recurso (sólo una parte)

- DELETE: elimina recursos del servidor o base de datos

PUT y PATCH son métodos de actualización. Analogía: PUT toda la bicicleta, PATCH solo la rueda

## get
Parámetros del método `get` del objeto `express`
1. Ruta `/`
2. Función anónima con parámetros: el `request` y el `response`

{{< highlight type="js" wrap="true" hl_lines="6-8">}}

import express from 'express';

const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
{{< /highlight >}}

En la página web debería visualizarse un `Hello World!`

## Reto
1. Crear un directorio nuevo 
2. Crear `index.js`
3. Inicializar npm
4. Instalar `express`
5. Escribir aplicación de servidor
6. Iniciar

## Cerrar puertos correctamente
En caso se tenga conflictos con los puertos, debemos saber [cómo cerrarlos correctamente](https://dev.to/sylwiavargas/how-to-properly-close-a-port-2p36)

## Anlizar el request

{{< highlight type="js" wrap="true" hl_lines="4">}}
...
app.get('/', (req, res) => {
  res.send('Hello World!')
  console.log(req);
})
...
{{< /highlight >}}

Al imprimir `req` se puede ver todo el cuerpo del request. Para hacerlo mas legible imprimir sólo sus `rawHeaders`

{{< highlight type="js" wrap="true" hl_lines="4">}}
...
app.get('/', (req, res) => {
  res.send('Hello World!')
  console.log(req.rawHeaders);
})
...
{{< /highlight >}}

## send
El método `send` del objeto `req` se usa para enviar una respuesta al _request_. Ej - se puede enviar texto plano `Hello world!` o HTML `<h1>hello h1<h1/>`

{{< highlight type="js" wrap="true" hl_lines="2">}}
app.get('/', (req, res) => {
  res.send('<h1>hello h1<h1/>')  
})
{{< /highlight >}}

## Nodemon: live reload
Para que cada que se modifique el código no se tenga que parar y volver a correr `node index.js`

1. Instalar `npm i -g nodemon` (-g instala global)
2. `nodemon index.js`
3. Probar cambiando y viendo cambios

## Endpoints (rutas)

Agregar los endpoints `/contact` y `/about`

{{% expand title="solución"%}}
```js
...
app.get('/contact', (req, res) => {
  res.send('<h1>contacto<h1/>')  
})

app.get('/about', (req, res) => {
  res.send('<h1>acerca de<h1/>')  
})
...
```
{{% /expand %}}