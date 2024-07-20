+++
archetype = "chapter"
title = "7. Reto"
weight = 7
+++

## Reto
Construir el backend con `express` usando todo lo aprendido a partir del código: 

{{< highlight type="html" wrap="true" title="index.html">}}
<!DOCTYPE html>
<html>

<head>
  <title>Secrets</title>
</head>

<body>
  <h1>Secrets</h1>
  <form action="/check" method="POST">
    <label for="password">Password:</label>
    <input type="text" id="password" name="password" required>
    <input type="submit" value="Submit">
  </form>
</body>

</html>
{{< /highlight >}}

{{< highlight type="html" wrap="true" title="secret.html">}}
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Secrets</title>
</head>

<body>
  <h1>Secrets</h1>
  <ul>
    <li>When making chocolate desserts, always add a little salt. It makes the chocolate flavour more intense.</li>
    <li>Whenever you travel abroad bring a new soundtrack for each place you visit, preferably one you have never heard
      before.
      In the future, every time you listen to each soundtrack again they will bring you vivid memories of the places you
      have
      visited.</li>
    <li>Do not try to be the person your parents would want you to be. Be the person you would like your child to be be.
      It more
      clearly
      defines your own convictions, desires, goals, and motivates you to be your best.</li>
    <li>If a friend or a family member gets diagnosed with dementia or alzheimer, in the early stages try to find out
      what their
      favorite songs of all time are. In this way you would be able to create a playlist for them that could be of great
      benefit in the later stages of the disease.</li>
    <li>When a friend is upset, ask them one simple question before saying anything else: 'Do you want to talk about it
      or do
      you want to be distracted from it?'</li>
    <li>If you ever forget your WiFi password or you want to get your school WiFi password etc. Just type this command
      into the
      command line of a computer already connected to that WiFi: netsh wlan show profile WiFi-name key=clear</li>
  </ul>

</body>

</html>
{{< /highlight >}}

- Funcionalidad: al escribir el password `ILoveProgramming` redireccionar a `secrets.html`
- Lidiar con paquetes no instalados, EMS/CJS
- `index.html` y `secrets.html` en la carpeta `public`
- Usar rutas externas y dinámicas en `sendFile`
- Usar `body-parser`
- Usar un middleware propio para el acceso mediante password

## Solución
{{< highlight type="js" wrap="true" title="index.js">}}
import express from "express";
import bodyParser from "body-parser";
import { dirname } from "path";
import { fileURLToPath } from "url";

const __dirname = dirname(fileURLToPath(import.meta.url));

const app = express();
const port = 3000;

app.use(bodyParser.urlencoded({ extended: true }));

var userIsAuthorized = false;

function passwordCheck(req, res, next) {
  if(req.body["password"] == "ILoveProgramming"){
    userIsAuthorized = true;
  }
  next();
}

app.use(passwordCheck);

app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

app.post("/check", function (req, res) {
  var password = req.body["password"];
  if (userIsAuthorized) {
    res.sendFile(__dirname + "/public/secret.html");
  } else {
    res.sendFile(__dirname + "/public/index.html");
  }
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
{{< /highlight >}}
