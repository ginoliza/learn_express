+++
archetype = "chapter"
title = "4. Postman"
weight = 4
+++

Para que el lado del cliente se comunique con el lado del servidor se usa HTTP requests, se puede enviar requests y el servidor puede responder con texto plano o HTML. Además puede responder con códigos de estado (ej: 404)

## Códigos de estado
Indican si un request HTTP ha sido completado correctamente

### Clases
- 1** Respuesta informativa
- 2** Respuesta de éxito
- 3** Respuesta de reidrección
- 4** Respuesta de error de cliente
- 5** Respuesta de error de servidor

[Documentación](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

## Ejercicio
Ir al navegador, abrir las herramientas de desarrollador y la pestaña de red `Network` e intentar acceder a `googl.com`

Se puede ver que al intentar acceder el estado es `301 Moved Permanently`, se retorna una nueva URL (esta vez correcta) y redirecciona

## Postman
Usualmente un request se realiza desde un sitio web mediante un formulario. Postman permite probar el backend sin tener un frontend

1. Correr el servidor con nodemon
1. Probar las 5 rutas distintas con Postman (abriendo una pestaña separada por cada request)
1. Verificar que para cada ruta se obtenga el código de estado correcto retornado del servidor
1. No se deberia obtener ningun código de estado 404 o 500

{{< highlight type="js" wrap="true">}}
import express from "express";
const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("<h1>Home Page</h1>");
});

app.post("/register", (req, res) => {
  //Do something with the data
  res.sendStatus(201);
});

app.put("/user/angela", (req, res) => {
  res.sendStatus(200);
});

app.patch("/user/angela", (req, res) => {
  res.sendStatus(200);
});

app.delete("/user/angela", (req, res) => {
  //Deleting
  res.sendStatus(200);
});

app.listen(port, () => {
  console.log(`Server started on port ${port}`);
});

{{< /highlight >}}