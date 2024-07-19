+++
archetype = "chapter"
title = "2. Servidor express"
weight = 2
+++

## Backend

Usualmente consiste de:
- Un servidor
- Una aplicación
- Una base de datos (opc)

El usuario se comunicará a traves de internet con el servidor realizando _requests_. Hay una aplicación escuchando esos _requests_ para poder servir al usuario. 

- Client side: frontend
- Server side backend

## Crear un servidor Express
1. Crear un directorio
2. Crear el archivo `index.js`
3. Inicializar el proyecto con `npm init`
4. Instalar el paquete `express`
5. Escribir una aplicación de servidor en `index.js`
6. Iniciar servidor

## listen

El método `listen` del objeto`express` toma como primer parámetro el puerto y una función callback que será disparada cuando el servidor inicia en el puerto.

{{< highlight type="js" wrap="true" >}}
import express from 'express';
const app = express()
const port = 3000

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
{{< /highlight >}}

Obtenemos `Cannot GET /` ya que no podemos obtener la ruta `/`

## lsof, netstat
Se puede saber qué puertos estan escuchando por alguna interacción:
- Windows: `netstat -ano | findstr "LISTENING"`
- Mac/Linux: `sudo lsof -i -P -n | grep LISTEN`