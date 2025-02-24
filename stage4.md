# Configuración de cliente y servidor para producción

Tu aplicación de Express debe retornar un archivo `index.html` desde su directorio `/public`, donde React creará la SPA que conformará el cliente. Para ello, es necesario configurar el servidor a este efecto, así como deshabilitar la gestión de errores 404 y 500 que, en adelante, será asumida por React.


## Variables de entorno en cliente

### ALTERNATIVA 1
El cliente debe tomar, para todos los servicios, el base URL `http://localhost:5000/api` durante el trabajo en local, mientras que la versión de prducción debe apuntar a `https://donuts-planet.herokuapp.com/api`.

Para esto usaremos [las variables de entorno de Create React App](https://create-react-app.dev/docs/adding-custom-environment-variables/). Modifica los scripts de React en el `package.json`, incluyendo la misma variable para que apunte a una u otra URL base según el entorno:

````json
 "scripts": {
    "start": "REACT_APP_BASE_URL=http://localhost:5000/api react-scripts start",
    "build": "REACT_APP_BASE_URL=https://donuts-planet.herokuapp.com/api react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
````
Asimismo, recuerda modificar todos los servicios para que tomen `process.env.REACT_APP_BASE_URL` como BaseURL.

### ALTERNATIVA 2. Que hacer si las variables de entorno no funcionan en windows.
Instalar la dependencia [*cross-env*](https://www.npmjs.com/package/cross-env)

Modificar los scripts del package.json para incluir el comando cross-env.


````json
 "scripts": {
    "start": "cross-env REACT_APP_BASE_URL=http://localhost:5000/api react-scripts start",
    "build": "cross-env REACT_APP_BASE_URL=https://donuts-planet.herokuapp.com/api react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
````
## ALTERNATIVA 3. Muchas variables de entorno o fallo de todo lo anterior.

Instalar la dependencia [*dotenv-cli*](https://www.npmjs.com/package/dotenv-cli)

En la raiz del proyecto:
 - Crear un .env.local con las variables de entorno locales como REACT_APP_BASE_URL=http://localhost:5000/api
 - Crear un .env.prod con las variables de entorno de producción como REACT_APP_BASE_URL=https://donuts-planet.herokuapp.com/api

En el packaje.son modifica los scripts de React incluyendo el comando dotenv para cargar el archivo correspondiente.
````json
 "scripts": {
    "start": "dotenv -e .env.local react-scripts start",
    "build": "dotenv -e .env.prod react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
````

## Build de producción 

Tu aplicación de React debe ser comprimida a su versión de producción e incluida en el directorio `/public` del servidor:

1. Accede al directorio `/client` de tu proyecto y ejecuta el comando `npm run build`. Esto creará un directorio `/build` con la versión de tu cliente comprimida y lista para subir a producción. 
2. Mueve todo el contenido de este nuevo directorio a `/public` en tu servidor.


## Configuración de servidor

Para que tu servidor pueda enviar archivos al front, es necesario incluir tanto un par de middlewares en `app.js`, lo que permitirá enviar siempre el `index.html` que incluye el build de React al cliente. Es importante que el middleware de configuración se haga antes que las rutas, mientras que el envío al index.html va después de ellas:

       const path = require('path')
       app.use(express.static(path.join(__dirname, "public"))) // MIDDLEWARE DE CONFIGURACIÓN
       
       require("./routes")(app) // AQUÍ VUESTRA CONEXIÓN A LAS RUTAS, YA DEBERÍAS TENERLA
       
       app.use((req, res) => res.sendFile(__dirname + "/public/index.html")); // MIDDLEWARE DE ENVÍO AL HTML
  
Asimismo, la gestión de errores 500 ya está siendo asumida por los propios endpoints del servidor, por lo que el archivo `error-handlers.config.js` de tu directorio `/config` ya no es necesario. Elimina tanto el archivo como su requerimiento en `app.js`.

Respecto a los errores 404, React los asumirá. Infórmate sobre [cómo gestionar errores 404 desde React Router](https://naveenda.medium.com/creating-a-custom-404-notfound-page-with-react-routers-56af9ad67807).

