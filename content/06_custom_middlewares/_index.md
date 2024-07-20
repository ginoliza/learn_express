+++
archetype = "chapter"
title = "6. Middlewares propios"
weight = 6
+++

## Morgan

Categorías de middleware:
- Preprocesamiento `body-parser`
- Autenticación
- Errores
- Logging `morgan`: sirve 

1. Instalar morgan `npm i morgan`
1. Importar
1. Usar

{{< highlight type="js" wrap="true">}}
import morgan from "morgan";
...
app.use(morgan("dev"));
...
{{< /highlight >}}

Imprime el log por terminal en el formato especificado dentro de `morgan(<formato>)`, 

## Crear propio middleware

Declarar una función con parámetros: `req, res, next` y usarla dentro de `app.use`. No olvidar `next` al final de la función, sino la página no cargará

{{< highlight type="js" hl_lines="6-10 12">}}
import express from "express";

const app = express();
const port = 3000;

function logger(req, res, next){
  console.log(req.method);
  console.log(req.url);
  next();
}

app.use(logger);

app.get("/", (req, res) => {
  res.send("Hello");
});

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
{{< /highlight >}}

## Mezclar Middlewares
Reto:
1. Enviar `/public/index.html` a la ruta GET `/`
1. Usar `body-parser` y un propio middleware para combinar ambos campos en uno y agregar el emoji ✌️
1. Enviar a la ruta `/submit`

{{< highlight type="js" wrap="true" lineNos="true" title="index.js" hl_lines="3-6 12-20 23 28">}}
import express from "express";
import { dirname } from "path";
import { fileURLToPath } from "url";
import bodyParser from "body-parser";

const __dirname = dirname(fileURLToPath(import.meta.url));

const app = express();
const port = 3000;
var bandName = "";

app.use(bodyParser.urlencoded({ extended: true }))

function bandNameGenerator(req, res, next){
  console.log(req.body);
  bandName = req.body["street"] + req.body["pet"];
  next();
}

app.use(bandNameGenerator);

app.get('/', (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
})

app.post("/submit", function (req, res) {
  console.log(req.body);
  res.send(`<h1>Your band name is: </h1><h2>${bandName}✌️</h2>`);
});

app.listen(port, () => {
  console.log(`Listening on port ${port}`);
});
{{< /highlight >}}

Es muy importante que `app.use(bodyParser.urlencoded({ extended: true }))` este antes de la función del propio middleware ya que se quiere utilizar `req.body`