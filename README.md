# Deploy a Heroku

## Objeto del documento

Describir el proceso para deplegar a producción en [Heroku](https://www.heroku.com/) + [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) una MERN Stack app con cliente [`create-react-app`](https://create-react-app.dev/docs/getting-started/) y API [ExpressJS](https://expressjs.com/).

El objetivo final es pasar a producción (despliegue, _deploy_, etc) nuestra aplicación local: disponer de una aplicación remota de Express que, a la vez que mantiene sus funcionalidades REST, sirva un archivo `index.html` donde React realice la composición del cliente. 

La aplicación de React estará contenida en el directorio `/public` del servidor.

## Objetivo del deploy

- **Base de datos**: hacer el paso a producción de la base de datos local a una base de datos remota en MongoDB Atlas:
  - Realizar el registro en MongoDB Atlas y crear la base de datos remota (stage 1)
  - Exportar los datos de la base de datos local e importaros en la base de datos remota (stage 2)
- **Aplicación Heroku**: crear en Heroku la aplicación remota (stage 3)
- **Setup en sevidor y _biuld_ en cliente**: configurar servidor y cliente para paso a producción (stage 4)
- **Paso a producción**: transferir a la aplicación de Heroku los archivos (stage 5)


## Fases de paso a producción

- Stage 1: [Registro en MongoDB Atlas y configuración base](https://github.com/ta-web-mad/deploy-mern-app/blob/main/stage1.md)
- Stage 2: [Paso a producción: base de datos](https://github.com/ta-web-mad/deploy-mern-app/blob/main/stage2.md)
- Stage 3: [Registro en Heroku y creación de aplicación remota](https://github.com/ta-web-mad/deploy-mern-app/blob/main/stage3.md)
- Stage 4: [Configuración de cliente y servidor para producción](https://github.com/ta-web-mad/deploy-mern-app/blob/main/stage4.md)
- Stage 5: [Paso a producción](https://github.com/ta-web-mad/deploy-mern-app/blob/main/stage5.md)

