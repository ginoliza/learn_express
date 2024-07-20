+++
archetype = "chapter"
title = "5. Middleware de express"
weight = 5
+++

El 'hombre al medio' de un request y los _handlers_, puede:
- Preprocesar el request: cambiar aspectos del request
- _Logging_: cuanto demoró, qué tipo es, su estado, etc
- Autenticación: ver si el request vino de alguien autorizado
- Procesar errores: identificar y manejarlos

## Archivos HTML
Al igual que es posible enviar texto plano o html en `send`, es posible enviar archivos HTML enteros especificando el URL del archivo

{{< highlight type="js" hl_lines="2-3 5 11" title="index.js">}}
import express from "express";
import { dirname } from "path";
import { fileURLToPath } from "url";

const __dirname = dirname(fileURLToPath(import.meta.url));

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});

{{< /highlight >}}

`index.html` se encuentra en la carpeta `public` del mismo directorio

{{< highlight type="html" wrap="true" title="index.html">}}
... html

<body>
  <h1>Band Name Generator</h1>
  <form action="/submit" method="POST">
    <label for="street">Street Name:</label>
    <input type="text" name="street" required>
    
    <label for="pet">Pet Name:</label>
    <input type="text" name="pet" required>
    
    <input type="submit" value="Submit">
  </form>
</body>

</html>
{{< /highlight >}}

## body-parser
Middleware de `nodejs` para parsear `body`s

Al usar `body-parser` se agrega la propiedad `body` al objeto `req` para poder acceder a los datos enviados por en el formulario de `index.html`

1. Instalar `body-parser` con `npm`
1. Importarlo
1. Usar `body-parser` como middleware con el método `use` del objeto `express` con el parámetro `bodyParser.urlencoded` ya que se manipulará un formulario HTML; se debe pasar la opción `extended` con `true` o `false` (casi siempre `true`). Usarlo ANTES de las rutas

Si dejo de usar `body-parser` como middleware el `body` del `request` no existe, y se obtiene `undefined`

{{< highlight type="js" wrap="true" hl_lines="2 4">}}
...
import bodyParser from "body-parser";
...
app.use(bodyParser.urlencoded({ extended: true }));
...
{{< /highlight >}}

## body

Se agrega la propiedad `body` al request `req` para poder manipular los datos

{{< highlight type="js" wrap="true" hl_lines="3">}}
...
app.post("/submit", function (req, res){
  console.log(req.body);
});
...
{{< /highlight >}}

Son valores `key value` por lo tanto se puede acceder a las entradas individuales:

{{< highlight type="js" wrap="true" hl_lines="3">}}
...
app.post("/submit", function (req, res){
  console.log(req.body["pet"]);
});
...
{{< /highlight >}}

Probar desde el sitio web y con Postman desde la pestaña Body con x-www-form-encoded. Desde Postman se puede enviar cualquier valor `key value`, desde la página web se tendría que modificar el `index.html`