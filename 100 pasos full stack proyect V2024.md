## Bienvenido a este tutorial práctico Full-Stack.

*Antes que nada, si lo abriste y se ve con títulos azules y todo sin mucho formato,
te invito a que abras el archivo en tu vscode con "boton derecho sobre el mismo" + "Open Preview"

Ahora si...

Hoy vamos a crear tanto un front como un back end con las siguientes
tecnologias: React,Flux,Node,Js,HTML,CSS./Flask,Python,JWT, SQLAlchemy.
Para asegurarnos que todo va a marchar bien ya tené instalado lo siguiente:
Git, Node, Visual Studio Code y Pyhton en tu computadora.

**Que no te gane la pereza y seguí leyendo esta intro que es importantísima.**
```bash
Teniendo en cuenta que este tutorial también va a incluir **deploy** de back y front,
te recomiendo que hagas el tutorial completo hasta el deploy antes de hacer cosas extra encima del proyecto, para asi poder testear si todo salió bien. O sea, seguí todos los pasos sin inventar mucho en un principio y al final del tutorial implementás tu magia sobre el proyecto.

Esto es más que nada porque, por ejemplo, en el deploy de back-end, la plataforma ( Render ) tiene
sus caprichos respecto a las versiones de dependencias que acepta.

( aclaro que Render, cuando deployamos un back, instala dependencias al igual que nosotros en nuestra computadora para poder hacer correr todo lo que nuestro back-end necesite ), entonces...

Render tiene una biblioteca interna de dependencias disponibles y un número de versiones que no es total. Esto hace que cuando se agrega una dependencia nueva al archivo pipfile, ( el pseudo package.json del backend ), pocas veces se pueda utilizar la última versión de la misma corriendo ok en el deploy online de Render.

Lo que quiero decir es que no hagas que tu backend dispare rayos laser por los ojos o tenga
muchas dependencias extra previo a hacer el deploy.

El deploy que enseño en este tutorial ya tiene el archivo pipfile completo con las dependencias 
utilizadas en este paso a paso. Si agregás otras dependencias antes del deploy, cuando hagas el deploy en render, es muy probable que aparezcan errores que pueden ser:

     - No encuentra el modulo correspondiente ( o sea que no esta incluido en el archivo pipfile )
     - La versión de la dependencia no es soportada por render ( en este caso render te muestra en su error las versiones soportadas )( se entiende que en este caso la dependnecia está escrita en pipfile pero la version no es soportada por render )

En cualquier caso este tutorial abarcará esa situación para que lo soluciones de manera sensilla en el paso número 87.
```
### Aclarado eso, vamos a dividir el proyecto en 4 partes:
    - La primera, del front-end, la crearemos automáticamente cuando creemos el proyecto via terminal. ( espera al paso 3 )
    - La que corresponde a back-end la crearemos como una carpeta llamada "api" dentro de "src" del proyecto react al finalizarlo.
    - La tercer parte será el deploy en Render del back-end.
    - Y por último el deploy del front-end.

### Índice:

     0/8   - Creación de proyecto react
     9/29  - Rutas
     30/40 - Flux/Estados Globales
     41/50 - Creación back-end + database
     51/58 - Iniciamos front y back
     59/68 - Frontend para el registro
     69/73 - Solucionamos problemas "CORS"
     74/74 - Test de conección / pedido de registro de usuario.
     75/86 - Deploy en render del back-end.
     87/87 - Solución de errores de deploy por falta de módulos o versiones no soportadas.
     88/90 - Chequeo de funcionalidad del back-end deployado.
     91/100- Deploy front-end

*>>>>Último tip antes de comenzar<<<<*: 

     Activá el auto guardado de tu VsCode para no olvidarte de guardar cambios. Como?
     Vas a "File"/Click en "AutoSave" y tiene que quedar marcardo.( esta maniobra puede ahorrarte horas ).

**0** - Creamos una carpeta que va a contenerlo todo.Abrir carpeta con vs.code.

**1** - Abrimos la terminal ( ctrl + ñ )/(menú terminal/new terminal)

**2** - Actualmente,en la terminal, estamos parados en la carpeta que creamos en el punto "0" (chequear que sea así).

**3** - Dentro de esta carpeta vamos a inicializar un nuevo proyecto React ( En el caso que no te deje, comprobar si tenes instalado node con "node --version", si no lo tenés, instalalo. Y si sigue sin dejarte hacer el create react, instala : "npm install -g create-react-app
"):

    npx create-react-app my-proyect

**4** - Entramos la carpeta del proyecto:
     
     cd .\my-proyect\

**5** - Testeamos si funciona:

     npm start

   Si podés, dejá abierto el browser

**6** - Debería compilar sin problemas. Ahora dejamos esta terminal, con el último mensaje "webpack compiled successfully", abierta para poder seguir trabajando desde otra ventana de comandos.

**7** - Abrimos otra terminal con el "+" que está a la derecha arriba de la terminal actual.

**8** - En esta terminal nueva nos volvemos a posicionar en la carpeta del proyecto:

    cd .\my-proyect\

**9** - Vamos a instalar ahora las dependencias para poder tener rutas en nuestro proyecto react. Están aclaradas las versiones compatibles con la instalación de react. Si no las aclarás instala las últimas disponibles y dan error.
     
    npm install react-router@6.14.2
    npm install react-router-dom@6.14.2

**10** - Una vez completas las instalaciones vamos a tocar un poco de código:

- a - Desde el menú lateral izquierdo que nos muestra las carpetas navegamos dentro de
    **my-proyect/src**

- b - Una vez expandido "src" damos boton derecho sobre el nombre de la carpeta y hacemos
    click en "nuevo archivo".Vamos a llamarlo **Layout.jsx**

- c - Le damos un mínimo formato de componente funcional. Ej:
```jsx
        import React from 'react'

        const Layout = () => {
          return (
               <div>Layout</div>
          )
        }

        export default Layout
```

**11** - En la carpeta "src" encontraremos un archivo "index.js". Ingresamos en el.

**12** - En las importaciones de este archivo reemplazamos:
     
     "import App from './App';"
     por
     "import Layout from './Layout';"

**13** - En la linea 10 del mismo archivo podemos ver como todavía se llama a:


     <App />
     Lo reemplazamos por:
     <Layout />

**14** - Si me hiciste caso en el paso 5 y dejaste abierto el browser que te mostraba
     tu react app encendida, ahora podrás ver que solo dice "Layout" por el contenido
     que tiene tu componente con ese nombre que ahora se está mostrando como la base
     de tu aplicación. Si previamente cerraste el browser. Abrí una pestaña nueva e ingresá:

     http://localhost:3000

**15** - Ahora hacemos click derecho sobre la carpeta "src" y le damos a "carpeta nueva"
     la vamos a llamar "views"

**16** - Repetimos el click derecho sobre "src" y creamos otra carpeta con nombre "components"

**17** - Dentro de la carpeta "views" vamos a crear tres archivos jsx: "Home.jsx","Contact.jsx" y "NotFound.jsx"
     Importante que estén escritos con la primer letra en mayùscula.
     Estos archivos son necesarios para testear las rutas que implementaremos en breve.

**18** - Contenido de archivos:

- a - Contenido base de Contact.jsx:
```jsx
import React from 'react'

const Contact = () => {
  return (
    <div>Contact</div>
  )
}

export default Contact
```
- b - Contenido base de Home.jsx es:
```jsx
import React from 'react'

const Home = () => {
  return (
    <div>Home</div>
  )
}

export default Home
```
- c- Contenido base de NotFound.jsx es:
```jsx
import React from 'react'

const NotFound = () => {
  return (
    <div>NotFound</div>
  )
}

export default NotFound
```
**19** - Al archivo NotFound.jsx lo personalizamos a gusto informando que la página que buscan no se encuentra.

**20** - En "Layout.jsx",por debajo de la importación de react., incluimos la importación:

     import { BrowserRouter, Route, Routes } from "react-router-dom";


**21** - Importamos los views "Home.jsx","Contact.jsx" y "NotFound.jsx":

     import Home from './views/Home.jsx'
     import Contact from './views/Contact.jsx'
     import NotFound from './views/NotFound.jsx'

**22** - Ahora tenemos que utilizar las importaciones en el area del return.Dentro de los divs vamos a hacer una capa de BrowserRouter y otra de Routes, para por fin dentro ,definir cada una de las rutas.
El código de Layout.jsx, en su totalidad quedaría así:
```jsx
import React from 'react'
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from './views/Home.jsx'
import Contact from './views/Contact.jsx'
import NotFound from './views/NotFound.jsx'

const Layout = () => {
  return (
    <div>
        <BrowserRouter>
            <Routes>
                <Route exact path="/" element={<Home/>}/>
                <Route exact path="/contact" element={<Contact/>}/>
                <Route exact path="/*" element={<NotFound/>}/>
            </Routes>
        </BrowserRouter>
    </div>
  )
}

export default Layout
```
**23** - Podemos chequear el funcionamiento desde el web browser:

     localhost:3000/  *  Home
     localhost:3000/contact  *  Contact
     localhost:3000/cualquiercosa *  NotFound

**24** - Ahora vamos a crear un "navbar" simple para poder acceder a las rutas sin necesidad
     de tener que ingresar mediante la url.
     Primero creamos una carpeta "components" dentro de src y dentro creamos "Navbar.jsx".

**25** - La estructura base del mismo es:
```jsx
import React from 'react'

const Navbar = () => {
  return (
    <div>Navbar</div>
  )
}

export default Navbar
```
**26** - Tenemos que agregar una importación importante justo por debajo de la importación de react:

     import { Link } from 'react-router-dom';

**27** - Ahora, dentro del div, agregamos una lista de links a cada una de las rutas:
     Les dejo el código con el navbar simple terminado:
```jsx
import React from 'react'
import { Link } from'react-router-dom'

const Navbar = () => {
  return (
    <div>
        Navbar
        <li><Link to="/">Home</Link></li>
        <li><Link to="/contact">Contact</Link></li>
    </div>
  )
}

export default Navbar
```
**28** - Ahora nos falta utilizar el Navbar. Tenemos 2 opciones.

- a - Incluimos el navbar fijo desde el Layout importandolo e incluyendolo por dentro de "BrowserRouter", y hermano de "Routes" Ejemplo:
```jsx
import React from 'react'
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from './views/Home.jsx'
import Contact from './views/Contact.jsx'
import NotFound from './views/NotFound.jsx'
import Navbar from './components/Navbar.jsx'

const Layout = () => {
  return (
    <div>
        <BrowserRouter>
            <Navbar/>
            <Routes>
                <Route exact path="/" element={<Home/>}/>
                <Route exact path="/contact" element={<Contact/>}/>
                <Route exact path="/*" element={<NotFound/>}/>
            </Routes>
        </BrowserRouter>
    </div>
  )
}

export default Layout
```
- b - La segunda manera es importar en cada view el navbar.
De esta forma es más controlable donde se pueda o no ver.
Dejo el ejemplo importando y utilizando desde Contact:
```jsx
import React from 'react'
import Navbar from '../components/Navbar'

const Contact = () => {
  return (
    <div>
      Contact
      <Navbar />
    </div>
  )
}

export default Contact
```
*Con estas dos formas podemos tener un navbar que funcione para todas las vistas.
         Ojo, utilizamos una o la otra. Si utilizamos las dos vamos a tener el navbar repetido.

**29** - Usando "useNavigate"- 
     Imaginemos que no queremos o no podemos utilizar Link, porque necesitamos que,
     al realizarse algun tipo de acción del usuario
     que no es precisamente un click en un link, se dispare una navegación a otra parte.
     En este caso tenemos nuestro useNavigate para salvarnos.

PASOS PUNTUALES:

- a - Lo importamos en el componente donde lo vamos a usar: 
  
          "import { useNavigate } from 'react-router-dom';"

- b - Dentro de la funcion componente, y antes del return, ponemos :
  
      "const navigate = useNavigate();"

    c - Agregamos **"navigate('/ruta')"** en el handler que necesitemos para que navegue a un punto especifico cuando sea necesario.

    *Cuando voy a utilizar useNavigate? ( Cuando un usuario se loguea, envia un formulario, termina de hacer una tarea, etc...)

**30** - Flux - Global State. Asegurate que en la terminal estas parad@ en la carpeta "my-proyect" e instalamos la dependencia:

     npm install events

**31** - En la carpeta src creamos una carpeta nueva llamada "js" y dentro de ella otra llamada "store"

**32** - Dentro de "store" creamos el archivo "appContext.js".
     Dentro del mismo seran creados el "Context" y el "Provider",
     además de modularizar el contenido del estado Global
     al archivo "flux.js" que en breve crearemos.
     Les dejo el contenido del archivo:
```jsx
import React, { useState, useEffect } from "react";
import getState from "./flux.js";


export const Context = React.createContext(null);

const injectContext = PassedComponent => {
	const StoreWrapper = props => {
		//this will be passed as the contenxt value
		const [state, setState] = useState(
			getState({
				getStore: () => state.store,
				getActions: () => state.actions,
				setStore: updatedStore =>
					setState({
						store: Object.assign(state.store, updatedStore),
						actions: { ...state.actions }
					})
			})
		);

		useEffect(() => {
		}, []);


		return (
			<Context.Provider value={state}>
				<PassedComponent {...props} />
			</Context.Provider>
		);
	};
	return StoreWrapper;
};

export default injectContext;
```
**33** - En la misma carpeta "store" creamos "flux.js"
     Acá será el lugar donde nuestras funciones e informaciones Globales
     serán guardadas para ser accesibles desde todos los componentes.
     El archivo se verá así ( viene con ejemplos ):
```jsx
const getState = ({ getStore, getActions, setStore }) => {
	return {
		store: {
			personas: ["Pedro","Maria"]
		},
		actions: {

			exampleFunction: () => {
                    console.log("hola")
                    return
			},
		}
	};
};

export default getState;
```

**34** - Ahora tenemos que hacer algunos cambios en "Layout.jsx" para aplicar el Provider.
     Esto quiere decir que tenemos que rodear el componente padre "Layout" para que 
     todos sus hijos tengan acceso a el estado global.
     Son cuatro cambios chicos:
- a - Importamos injectContext de appContext.
- b - En la exportación final utilizamos injectContext("y ponemos el Layout como argumento")
- c - En BrowserRouter le damos atributo basename={basename}
- d - Elegimos un basename str vacio o desde .env:
  
          const basename = process.env.BASENAME || "";
     El código se verá así:
```jsx
import React from 'react'
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Home from './views/Home.jsx'
import Contact from './views/Contact.jsx'
import NotFound from './views/NotFound.jsx'
import Navbar from './components/Navbar.jsx'
import injectContext from "./js/store/appContext";

const Layout = () => {

  const basename = process.env.BASENAME || "";

  return (
    <div>
        <BrowserRouter basename={basename}>
            <Navbar/>
            <Routes>
                <Route exact path="/" element={<Home/>}/>
                <Route exact path="/contact" element={<Contact/>}/>
                <Route exact path="/*" element={<NotFound/>}/>
            </Routes>
        </BrowserRouter>
    </div>
  )
}

export default injectContext(Layout)
```

**35** - Es momento de consumir el estado global. Para ello vamos a ir a **Home.jsx**

**36** - Vamos a modificar el import react sumandole algo:

     import React, {useContext} from "react";

**37** - Vamos a importar Context de appContext:

     import { Context } from "../js/store/appContext.js";

**38** - Dentro del componente funcional, todavia el **Home.jsx**, vamos a hacer un destructuring del Context:

     const { store, actions } = useContext(Context)

Esto utiliza el contexto provisto desde Layout el cual nos conecta con flux.js

**39** - Probamos **store.personas** en el código de **Home.jsx**. Ejemplo del componente entero:
```jsx
import React, { useContext } from 'react'
import { Context } from "../js/store/appContext.js"

const Home = () => {

  const { store } = useContext(Context)

  return (
    <div>
      Home
      <h5>Personas en global:{store.personas}</h5>
    </div>
  )
}

export default Home
```
**40** - Si todo salió bien, mirando Home en el browser, deberías visualizar el siguiente texto:

     "Personas en global:PedroMaria"

## Felicitaciones. Ya tenes un proyecto react con rutas y estado global funcional.

# Ahora BACK-END

**41** - En la terminal donde iniciamos previamente "npm start", ahora tecleamos "ctrl + c"
     y confirmamos con "S"+"enter" para terminar la ejecución react.

**42** - Eliminamos la terminal actual pasando el mouse por encima de la misma a la derecha
     aparecerá un tacho de basura, click en el.
     (así limpiamos momentaneamente nuestro lugar de trabajo para no confundirnos)

**43** - Vamos a la carpeta "src"( desde el explorer izquierdo en vs code ) y creamos una
     nueva carpeta dentro llamada "api"
     Acá vamos a guardar todo el contenido back-end + database

**44** - Creamos un entorno virtual: 
     Un entorno virtual de Python sirve para accesar a
     las bibliotecas y los scripts instalados en él, y estos están aislados de los 
     instalados en otros entornos virtuales, y por defecto,tambien aislados de cualquier biblioteca 
     instalada en un «sistema» Python, es decir, uno que esté instalado como parte de tu sistema operativo.
     Entonces...
     Nos posicionados en la terminal:
     
     "my-proyect\src\api"
...y tipeamos:

	python -m venv myenv      # Crea unentorno virtual
	myenv\Scripts\activate   # Activa el entorno virtual

*(ACTIVA EL ENTORNO VIRTUAL CADA VEZ QUE SE INICIA EL VS CODE)

     
     Si al querer Activar el entorno virtual con "myenv\Scripts\activate" nos sale algo así:

     "No se puede cargar el archivo C:\Users\PC\Desktop\100 Pasos\my-proyect\src\api\myenv\Scripts\Activate.ps1 porque la ejecución de scripts está deshabilitada en
     este sistema."

     Se soluciona fácil entrando a powershell como administrador
     y tipeando:

     Set-ExecutionPolicy RemoteSigned

     Te preguntará si estas seguro y tenés que escribir "S" y darle al "Enter". Una vez hecho esto intentá nuevamente ejecutar tu entorno.

**45** - En la terminal, nos aseguramos de seguir parados en **"my-proyect\src\api"**
     Además de estar seguros que **(myenv)** está visible al principio de la ruta
     indicando que esta activada.
     Ahora instalamos librerias JWT, bcrypt,flask y sqlalchemy:

        pip install flask flask_jwt_extended flask_bcrypt flask_sqlalchemy

**46** - En la carpeta "api" creamos algunos archivos. Comenzando por "app.py". Este archivo contiene la inicialización del servidor. Es el corazón de back-end y viene
     con el siguiente contenido:
```python
import os # para saber la ruta absoluta de la db si no la encontramos
from flask_bcrypt import Bcrypt  # para encriptar y comparar
from flask import Flask, request, jsonify # Para endpoints
from flask_sqlalchemy import SQLAlchemy  # Para rutas
from flask_jwt_extended import  JWTManager, create_access_token, jwt_required, get_jwt_identity
from admin_bp import admin_bp                       # Acá importamos rutas admin
from public_bp import public_bp                     # Acá importamos rutas public
from database import db                             # Acá importamos la base de datos inicializada

app = Flask(__name__)


# ENCRIPTACION JWT y BCRYPT-------

app.config["JWT_SECRET_KEY"] = "valor-variable"  # clave secreta para firmar los tokens.( y a futuro va en un archivo .env)
jwt = JWTManager(app)  # isntanciamos jwt de JWTManager utilizando app para tener las herramientas de encriptacion.
bcrypt = Bcrypt(app)   # para encriptar password


# REGISTRAR BLUEPRINTS ( POSIBILIDAD DE UTILIZAR EL ENTORNO DE LA app EN OTROS ARCHIVOS Y GENERAR RUTAS EN LOS MISMOS )


app.register_blueprint(admin_bp, url_prefix='/admin')  # poder registrarlo como un blueprint ( parte del app )
                                                       # y si queremos podemos darle toda un path base como en el ejemplo '/admin'

app.register_blueprint(public_bp, url_prefix='/public')  # blueprint public_bp



# DATABASE---------------
db_path = os.path.join(os.path.abspath(os.path.dirname(__file__)), 'instance', 'mydatabase.db')
app.config['SQLALCHEMY_DATABASE_URI'] = f'sqlite:///{db_path}'


print(f"Ruta de la base de datos: {db_path}")


if not os.path.exists(os.path.dirname(db_path)): # Nos aseguramos que se cree carpeta instance automatico para poder tener mydatabase.db dentro.
    os.makedirs(os.path.dirname(db_path))

with app.app_context():
    db.init_app(app)
    db.create_all() # Nos aseguramos que este corriendo en el contexto del proyecto.
# -----------------------

# AL FINAL ( detecta que encendimos el servidor desde terminal y nos da detalles de los errores )
if __name__ == '__main__':
    app.run(debug=True)
```
Ahora creamos database.py con el siguiente contenido:

```python
# database.py
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

```
Seguimos creando models.py (todos estos archivos en la misma carpeta api !) con el siguiente contenido:
     
```python
from database import db

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50))
    email = db.Column(db.String(100), unique=True)
    password = db.Column(db.String(255))

```
Ahora vamos con admin_bp.py con el siguiente contenido:
     
```python
# En el conjunto de datos que quiero separar ( en este caso este tipo de rutas ), importo...
from flask import Blueprint, request, jsonify # Blueprint para modularizar y relacionar con app
from flask_bcrypt import Bcrypt                                  # Bcrypt para encriptación
from flask_jwt_extended import JWTManager, create_access_token, jwt_required, get_jwt_identity   # Jwt para tokens
from models import User                                          # importar tabla "User" de models
from database import db                                          # importa la db desde database.py
from datetime import timedelta                                   # importa tiempo especifico para rendimiento de token válido


admin_bp = Blueprint('admin', __name__)     # instanciar admin_bp desde clase Blueprint para crear las rutas.

bcrypt = Bcrypt()
jwt = JWTManager()

# RUTA TEST de http://127.0.0.1:5000/admin_bp que muestra "Hola mundo":
@admin_bp.route('/', methods=['GET'])
def show_hello_world():
     return "Hola mundo",200


# RUTA CREAR USUARIO
@admin_bp.route('/users', methods=['POST'])
def create_user():
    try:
        email = request.json.get('email')
        password = request.json.get('password')
        name = request.json.get('name')

        if not email or not password or not name:
            return jsonify({'error': 'Email, password and Name are required.'}), 400

        existing_user = User.query.filter_by(email=email).first()
        if existing_user:
            return jsonify({'error': 'Email already exists.'}), 409

        password_hash = bcrypt.generate_password_hash(password).decode('utf-8')


        # Ensamblamos el usuario nuevo
        new_user = User(email=email, password=password_hash, name=name)


        db.session.add(new_user)
        db.session.commit()

        good_to_share_user = {
            'id': new_user.id,
            'name':new_user.name,
            'email':new_user.email,
            'password':password
        }

        return jsonify({'message': 'User created successfully.','user_created':good_to_share_user}), 201

    except Exception as e:
        return jsonify({'error': 'Error in user creation: ' + str(e)}), 500


#RUTA LOG-IN ( CON TOKEN DE RESPUESTA )
@admin_bp.route('/token', methods=['POST'])
def get_token():
    try:
        #  Primero chequeamos que por el body venga la info necesaria:
        email = request.json.get('email')
        password = request.json.get('password')

        if not email or not password:
            return jsonify({'error': 'Email and password are required.'}), 400
        
        # Buscamos al usuario con ese correo electronico ( si lo encuentra lo guarda ):
        login_user = User.query.filter_by(email=request.json['email']).one()

        # Verificamos que el password sea correcto:
        password_from_db = login_user.password #  Si loguin_user está vacio, da error y se va al "Except".
        true_o_false = bcrypt.check_password_hash(password_from_db, password)
        
        # Si es verdadero generamos un token y lo devuelve en una respuesta JSON:
        if true_o_false:
            expires = timedelta(minutes=30)  # pueden ser "hours", "minutes", "days","seconds"

            user_id = login_user.id       # recuperamos el id del usuario para crear el token...
            access_token = create_access_token(identity=user_id, expires_delta=expires)   # creamos el token con tiempo vencimiento
            return jsonify({ 'access_token':access_token}), 200  # Enviamos el token al front ( si es necesario serializamos el "login_user" y tambien lo enviamos en el objeto json )

        else:
            return {"Error":"Contraseña  incorrecta"}
    
    except Exception as e:
        return {"Error":"El email proporcionado no corresponde a ninguno registrado: " + str(e)}, 500
    
# EJEMPLO DE RUTA RESTRINGIDA POR TOKEN. ( LA MISMA RECUPERA TODOS LOS USERS Y LO ENVIA PARA QUIEN ESTÉ LOGUEADO )
    
@admin_bp.route('/users')
@jwt_required()  # Decorador para requerir autenticación con JWT
def show_users():
    current_user_id = get_jwt_identity()  # Obtiene la id del usuario del token
    if current_user_id:
        users = User.query.all()
        user_list = []
        for user in users:
            user_dict = {
                'id': user.id,
                'email': user.email
            }
            user_list.append(user_dict)
        return jsonify(user_list), 200
    else:
        return {"Error": "Token inválido o no proporcionado"}, 401

```
Seguimos con public_bp.py con el siguiente contenido:
     
```python
# public_bp.py (Blueprint para las rutas públicas)
# Public está vacio para que lo llenes vos. Podes tener en cuenta las rutas de "admin_bp.py" Podrias hacer casi lo mismo acá.
# Si vas a hacer un copy paste, acordate que tenés que cambiarle los nombres a todas las rutas @admin_bp a @public_bp.
# Después no digas que no te avisé...

from flask import Blueprint

public_bp = Blueprint('public', __name__)

@public_bp.route('/')
def home():
    return 'Home Page'

@public_bp.route('/about')
def about():
    return 'About Page'
```

**47** - Hecho todo esto ahora tenemos que encender el server!:

     python app.py
*Si no funciona es porque no estas posicionado en la terminal en carpeta "api".

**48** - Si todo salió bien al encender, ahora tenes una carpeta nueva dentro de **"api"** llamada **"instance"**
     y dentro de ella tu archivo **"mydatabase.db"**

**49** - Entra a tu explorador web y en la url colocá:

     http://127.0.0.1:5000/admin

Deberia mostrarte el "Hola mundo"

**50** - Intentá utilizar las otras rutas con postman:

-

     a - Crear usuario:   POST
                          127.0.0.1:5000/admin/users
                         {
                              "name":"NombreFalso"
                              "email": "emailfalso2@gmail.com",
                              "password": "12345678"
                         }

-

     b - Loguear y obtener token:   POST
                                    127.0.0.1:5000/admin/token
                                   {
                                        "email": "emailfalso2@gmail.com",
                                        "password": "12345678"
                                   }

-

     c - Obtener datos con token:   GET 
                                    127.0.0.1:5000/admin/users
                                    headers{
                                        Authorization : Bearer <el-token-recibido-en-el-paso-anterior-sin-comillas>
                                    }


Perfecto. Ya tenemos nuestro back-end encendido y funcional.
Recordá que podes crear todas las rutas y modelos que quieras. 
Y sobre todo recordá que cuando vuelvas a levantar **"python app.py"** previamente actives: **"myenv\Scripts\activate"**
Podes tenerlo corriendo a la par de **"npm start"** del proyecto react y probar todo el flow junto.

**51** - Ahora vamos a iniciar back y front. Voy a suponer que cuando llegaste al paso 50 festejaste
     y cerraste todo el proyecto, por lo que ahora vamos a encarar los siguientes pasos desde la apertura.
     En tu explorador de archivos, de tu sistema operativo, buscá la carpeta de tu proyecto **"my-proyect"**, click de botón derecho sobre la misma y click en "abrir con VsCode".

**52** - Una vez abierto VsCode, abrimos la terminal ( ctrl + ñ )/(menú terminal/new terminal)

**53** - Ejecutamos: 

     npm start

**54** - Abrimos otra terminal con el "+" que está a la derecha arriba de la terminal actual.

**55** - En la nueva terminal nos posicionamos en:
     
     my-proyect\src\api

**56** - Activamos el entorno/myenv de python escribiendo:

     myenv\Scripts\activate

**57** - Ejecutamos app.py:

     python app.py

**58** - En este momento tenemos nuestro front corriendo en "http://localhost:3000/"
     y nuestro back en "http://127.0.0.1:5000/"
     Chequealo vos mism@ utilizando cada direccion url en tu web browser

**59** - Ahora vamos a implementar lógica en front-end para poder conectar ambos mundos.
     Iniciamos por crear el flow que va a crear un usuario en nuestro sitio.
     Para ello nececitamos crear una nueva view que llamaremos:
     
           **LoginRegister.jsx**

En VsCode, a la izquierda, nos posicionamos en la carpeta **my-proyect\src\views**
Click con botón derecho sobre ella / "new file" > **LoginRegister.jsx**

**60** - El contenido base de LoginRegister.jsx es:
```jsx
import React from 'react'

const LoginRegister = () => {
  return (
    <div>LoginRegister</div>
  )
}

export default LoginRegister
```
**61** - Ya creado el archivo tenemos que hacer que sea el primero en mostrarse al cargar nuestra url base. Cómo?

Abrimos el archivo **"Layout.jsx"**, hubicado en la carpeta "src".

**62** - En sus importaciones agregamos:

     import LoginRegister from './views/LoginRegister.jsx';

**63** - Ahora agregamos una "route" que lo muestre y que su path sea "/":

     <Route exact path="/" element={<LoginRegister/>}/>

**64** - No olvidemos cambiar el path de la ruta de home de "/" a "/home"

     <Route exact path="/home" element={<Home/>}/>

**65** - Volvemos a abrir "LoginRegister.jsx" que se encuenrta en carpeta "views".

 Agreguemos un formulario de "register" a LoginRegister.jsx

- a - Lo primero es preparar un estado local para completarse con
los datos que el usuario va a ir ingresando.
Para ello en la importación de react agregamos useState:
```jsx
          import React, { useState } from 'react'
```
- b - Creamos dentro de el componente funcional un estado local:
```jsx
      const [formData, setFormData] = useState({
        name: '',
        email: '',
        password: ''
      });
```
- c - Necesitamos un handler para modificar el estado local:
```jsx
      const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData({ ...formData, [name]: value });
      };
```
- d - Necesitamos preparar LoginRegister.jsx para consumir estado global. Entonces importamos:
```jsx
         import { Context } from '../js/store/appContext.js';
         import { useContext } from 'react';
```
- e - Dentro de el componente funcional traemos store y actions:
```jsx
         const { store, actions } = useContext(Context)
```  

- f - Necesitamos otro handler para enviar los datos al action:
```jsx
      const handleRegister = () => {
        const { name, email, password } = formData;
        // Llamada a la función de acción 'register' con los datos del formulario
        actions.register(name, email, password);
      };
```
- g - Como el contenido para hacer el formulario es extenso, te dejo el LoginRegister.jsx completo:
```jsx
import React, { useState, useContext } from 'react'
import { Context } from '../js/store/appContext.js';

const LoginRegister = () => {

    const { store, actions } = useContext(Context)

    const [formData, setFormData] = useState({
        name: '',
        email: '',
        password: ''
      });

    const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData({ ...formData, [name]: value });
    };

    const handleRegister = () => {
        const { name, email, password } = formData;
        actions.register(name, email, password);
      }; 

    return (
    <div>
        <h2>Register</h2>
        <h3>Test store: {store.personas}</h3>
        <form>
        <div>
            <label htmlFor="name">Nombre:</label>
            <input
            type="text"
            id="name"
            name="name"
            value={formData.name}
            onChange={handleChange}
            />
        </div>
        <div>
            <label htmlFor="email">Email:</label>
            <input
            type="email"
            id="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            />
        </div>
        <div>
            <label htmlFor="password">Contraseña:</label>
            <input
            type="password"
            id="password"
            name="password"
            value={formData.password}
            onChange={handleChange}
            />
        </div>
        </form>
        <button onClick={handleRegister}>Register</button>
    </div>
    );
};

export default LoginRegister;
```
**66** - Ahora tenemos que crear la funcion llamada "register" en el actions de flux.js:
```javascript
register: async (name, email, password) => {
        try {
          const data = {
            name: name,
            email: email,
            password: password
          };

          const response = await fetch("http://127.0.0.1:5000/admin/users", {
            method: "POST",
            headers: {
              "Content-Type": "application/json"
            },
            body: JSON.stringify(data)
          });

          const statusCode = response.status;
          const responseData = await response.json();

          if (statusCode === 201) {
            setStore({ ...getStore(), registerStatus: true });
          }

          return responseData;
        } catch (error) {
          console.error("Error:", error);
          throw error;
        }
}
```
**67** - Ojo que estamos modificando en el "store" una propiedad que todavia no agregamos!
     En store como propiedad hermana de "personas" agregamos **"registerStatus"** seteado en **false**:
```javascript
     store: {
			personas: ["Pedro", "Maria"],
			registerStatus: false
		},
```
**68** - Deberiamos ahora agregar en nuestro **LoginRegister.jsx** , una escucha del cambio de este estado global para poder informarle al usuario que su registro fué exitoso.
Entonces, justo después del botón de registro vamos a agregar la siguiente linea:
```jsx
     {store.registerStatus && <h4>Registro Exitoso</h4>}
```
*Este short circuit muestra el texto de h4 solo si store.registerStatus es true.

**69** - Por último, antes que te veas tentado a hacer click en register y veas el famoso error de **"CORS"**,
     vamos a solucionar ese problema de antemano.
     Desde el EXPLORER de carpetas a la izquierda del VsCode, abrimos el archivo app.py que se encuentra
     en my-proyect\src\api\app.py

**70** - Abrimos una terminal nueva con + ( ya que las otras 2 estan ejecutando back y front actualmente )
en la terminal nos posicionamos en my-proyect\src\api\ *No actives el "myenv"*
     
**71** - Instalamos dependencias flask_cors:

     pip install flask-cors

**72** - En el archivo app.py que abrimos previamente vamos a agregar las importaciones:

     from flask_cors import CORS

     *>PROBABLEMENTE se calló el back y no lo podes levantar porque da un problema para reconocer el modulo. Reiniciá el vscode y deberia solucionarse<

     *Si sigue sin reconocer intentá en la terminal, primero entrar en el entorno virutal,
     posicionado en /api , escibir:
     myenv\Scripts\activate
     y después instalar dependencias:
     pip install flask-cors:
     *Intenta nuevamente correr el servidor con "python app.py"

     *Si sigue sin reconocer, todavia con el entorno virtual abierto:
     pipenv install flask-cors

     *Si sigue con problemas después de los últimos pasos descriptos volver a reiniciar VsCode.

     (Ahora si o si te tendria que funcionar y reconocer flask_cors)

**73** - Por último encontrá en tu **app.py** la linea que contiene "app = Flask(__name__)"
     y por debajo de ella agregá:

     CORS(app, resources={r"/*": {"origins": "http://localhost:3000"}}, supports_credentials=True)

Esto deberia solucionar el problema de CORS.
     
*Si todavía tenes problemas de cors al intentar ejecutar tu registro en el browser, fijate en la url del fetch del action en flux.js,
     si se encuentra exactamente asi: "http://127.0.0.1:5000/users"
     Un error bastante común es agregarle una barra más al final ".../users/"

*Otro error de "CORS" puede aparecer si los nombres de las propiedades de un objeto enviado no son los mismos esperados por la lógica de la ruta en el A.P.I. ( MUCHO OJO CON ESO )

*Si seguis con el problema, verificá que tus puertos en visual studio code esten en "public" y no "private".
( ahora si te tiene que reee funcionar)

*Ya como último consejo, quitá las rutas duplicadas comentadas que tengas de respaldo en el mismo archivo. Por alguna razon Python a veces las interpreta. Si, inesperado. Por lo cual te recomiendo tener tu app.py limpio sin codigo residual comentado. ( esto puede generar algunos errores CORS o hasta error por multiples rutas o funciones llamadas de la misma forma )

**74** - Desde el web browser, completá el formulario de registro y dale al botón **"Register"**
     Esto deberia mostrar **"Registro Exitoso"** en la página, y además **"POST /admin/users HTTP/1.1" 201 -"**
     en la terminal del backend.

### Ahora si podes presumirle a todo el mundo que por fin conectaste un FRONT-END con un BACK-END
De acá en adelante te invito a realizar los pasos necesarios para poder hacer un login,
y por último hacer un pedido al back-end de los usuarios existentes utilizando el token
generado al momento de loguearte.

**75** - Que momento para estar vivo! Vamos a comenzar el proceso de DEPLOY en Render.

**76** - Vamos a https://www.github.com y:

 - a - Si no estas logueado, logueate.
 - b - Una vez logueado, click en tu foto arriba a la derecha y click en "Your repositories".
 - c - Click en el boton verde a la derecha que dice "New".
 - d - Selecciona tu propio usuario como Owner.
 - e - En el "Repository name", poné el nombre de tu proyecto. Ej: mi-proyecto-deployado.
 - f - Si estas inspirad@ dale una descripción ( opcional ).
 - g - Dejalo marcado en "Public".
 - h - Asegurate que "Add a README file" se encuentre **desmarcado**.
 - i - Dale a "Create repository".
 - j - En la pantalla que se abre copiate el comando que asigna la ruta de origen. Se deberia ver algo así: "git remote add origin "https://github.com/regenerik/mi-proyecto-deployado.git" pero con tu nombre de usuario.

**77** - Volvemos a nuestro VsCode y abrimos la terminal.

 - a - En la terminal nos posicionamos en \my-proyect\
 - b - Ejecutamos: git init
 - c - Ahora ejecutamos el comando que copiaste en el punto anterior **76-j**.
 deberia verse algo como : git remote add origin https://github.com/tuUsuario/mi-proyecto-deployado.git
 (si le pusiste otro nombre al proyecto tambien ese valor será diferente)

 **78** - Vamos a configurar el archivo .gitignore que se encuentra en \my-proyect\
          Te lo dejo entero y te explico que en el, estamos evitando que en los futuros commits
          subamos a github los archivos de los modules del front y los lib del back:
```bash
# See https://help.github.com/articles/ignoring-files/ for more about ignoring files.

# do not ignore gitpod and render files
!.gitpod.Dockerfile
!Dockerfile.render
!.gitpod.yml
!render.yml
!.render-buildpacks.json

# dependencies
/node_modules
/.pnp
.pnp.js
src/api/myenv/lib/

# testing
/coverage

# production
/build

# misc
.DS_Store
.env.local
.env.development.local
.env.test.local
.env.production.local

npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

**79** - Ahora que le dijimos a github que archivos tener en cuenta y cuales no, vamos a crear nuestro commit y subir nuestro repositorio local a la versión que creamos online. Como?

 - a - En la terminal ejecutamos: git status ( nos va a mostrar los archivos del proyecto en rojo )
 - b - Si no hay archivos de módulos en rojo ( te vas a dar cuenta porque son un montón si los hay ) significa que hicimos todo bien y ejecutamos:
          
          git add .

 - c - Ahora:
 
          git commit -m "mi primer commit del proyecto"

 - d - Sigue con:
 
          git push
     ( puede que suba los cambios o que te pida establecer a que rama )

 - e - Si no subió con "git push" intenta con :

          git push --set-upstream origin master

 - f - Asegurate que se enviaron los paquetes y que tu repo online tiene toda la información.

**80** - Es la hora de hacer algunas modificaciones en nuestro archivo app.py:

 - a - En la linea de código del CORS que actualmente está así:

          CORS(app, resources={r"/*": {"origins": "http://localhost:3000"}}, supports_credentials=True)
     
     Lo modificás para que sea igual al siguiente ejemplo:

          CORS(app, resources={r"/*": {"origins": "*"}}, supports_credentials=True)
     
     De esta manera permitimos que cualquier origen de pedidos al back-end sea permitido. Esto es escencial para que podamos hacer test con nuestro POSTMAN. Mas adelante por motivos de seguridad vamos a cambiar el valor de "origins" por la url de nuestro front end deployado para que solo desde el se puedan hacer pedidos a nuestro back-end.

 - b - Cerca del final de nuestro archivo app.py vamos a encontrar algo como:

          if __name__ == '__main__':
               app.run(debug=True)
     
     Lo modificás para que sea igual al siguiente ejemplo:

          if __name__ == '__main__':
               app.run(debug=True, host='0.0.0.0', port=5000)

     Permitimos de esta manera que se acepte cualquier host desde el puerto 5000
     A partir de ahora podemos dejar en paz el archivo app.py

**81** - Para evitar problemas de modulos a la hora de hacer el deploy tenemos que tocar también el archivo pipfile que se encuentra en **my-proyect\src\api**

Ojoo! No vamos a modificar a mano el **Pipfile.lock**, vamos a modificar el archivo: **Pipfile**.
Si no tenés el archivo **Pipfile** ni el **Pipfile.lock**, no te desesperes. Creá el archivo **Pipfile** dentro de la carpeta /api/ (y no se pone extensión. solo Pipfile para crearlo respetando la mayúscula)
Dejo el contenido del archivo Pipfile con los packages y versiones compatibles con Render:
```bash
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
flask = "==2.2.5"
flask-cors = "*"
flask-bcrypt = "*"
werkzeug = "==2.2.3"
flask_sqlalchemy = "*"
flask_jwt_extended = "*"
importlib_metadata = "==6.7.0"
zipp = "==3.15.0"

[dev-packages]

[requires]
python_version = "3.11"
```
**82** - Ahora que tenemos el pipfile modificado tenemos que hacer algunos pasos para que el pipfile.lock también se ve afectado por estos cambios. Pero esto lo hacemos desde la terminal:

 - a - Abrimos una terminal nueva con el "+" ( **no actives el myenv** )
 - b - Nos posicionamos en src/api/
 - c - Ejecutamos:

          pipenv lock --clear

 - d - Ejecutamos:

          pipenv install

 Esto debería re escribir el contenido de pipfile.lock respecto a las dependencias mencionadas en el pipfile común.

**83** - Ahora que tenemos los archivos pipfile y pipfile.lock modificados tenemos que subirlos a nuestro repo online:
 - a - Nos posicionamos en la terminal.

 - b - Ejecutamos git status para asegurarnos que esos dos archivos están cambiados:

          git status

 - c - Agregamos los cambios al proximo commit:

          git add .

 - d - Creamos el commit que contendrá los cambios:

          git commit -m "update pipfile and pipfile.lock"

 - e - Pusheamos el commit:
     
          git push

Si todo salió bien deberiamos tener todos estos cambios disponibles en nuestro proyecto de github.
( estos cambios son necesarios para poder deployar en los próximos pasos ).

**84** - Hora de ir a https://render.com/ y registrarse:

 - a - Click en "SIGN IN" localizado en el navbar arriba a la derecha.
 - b - Elegí Googe para registrarte.
 - c - Elegí la cuenta con la que queres registrarte.
 - d - Click en "COMPLETE SIGN UP"
 - e - Abrí tu correo y confirmá tu e-mail con el link que te envian.

**85** - Ahora que estas registrado en Render vamos a preparar nuestro deploy. Si hiciste todo bien hasta el último paso de el punto **84** debes estar parado en : https://dashboard.render.com/
Si lo cerraste lo podes volver a abrir desde el link que te dejé.

- a - Click en el botón azul del navbar que dice "New+"
- b - Elegir "Web Service"
- c - Ahora que estas parado en "Create a new Web Service" te va a pedir que conectes tu github.
- d - Una vez otorgados los permisos para conectarse con github te pregunta si queres importar todos los repositorios o alguno en específico. Traé solo el específico correspondiente al proyecto.
- e - En la misma vista aparecerá tu repositorio y un botón azul "Conect". Click en el. ( si no aparece fijate mas abajo que podes pegar el link del repo público y salir andando desde ahí.)

**86** - Nos encontramos en una ventana donde tendremos que rellenar algunos campos para configurar nuestro "web service". Te paso todos los parámetros para rellenar:

- Name : test-deploy-back-on-render
- Region: oregon
- Branch: master
- Root directory: src/api
- Runtime: Python 3
- Build command: pipenv install --system --deploy
- Start command: python app.py
- Instance type: free

Finalmente click en "CREATE WEB SERVICE".

**87** - En este momento se está creando tu deploy. Si no me hiciste caso y te emocionaste instalando otras dependencias en el back-end,probablemente te de error durante la ejecución.
Como mencioné al principio de este curso es posible que tengas dos tipos de errores:

     1 - No encuentra el modulo correspondiente ( o sea que no esta incluido en el archivo pipfile ).
     Podés reconocer este problema si en el error de el deploy te aparece algo como:
     "MODULE tu_modulo NOT FOUND" fail to build".
     En este caso en particular tendrías que ir al archivo pipfile y agregar el módulo faltante a la sección de [packages]. Actualmente tu [packages] contiene:

          flask = "==2.2.5"
          flask-cors = "*"
          flask-bcrypt = "*"
          werkzeug = "==2.2.3"
          flask_sqlalchemy = "*"
          flask_jwt_extended = "*"
          importlib_metadata = "==6.7.0"
          zipp = "==3.15.0"

          Deberias agregar por debajo del último:

          tu_modulo_faltante = "*"

          Una vez modificado pipfile tenemos que seguir los pasos 82 y 83 para actualizar pipfile, pipfile.lock y que queden online en nuestro github.
          Finalmente volvemos a la pagina de "Render" y en la misma vista donde tuvimos el error de deploy, tendremos un botón "Manual deploy". Click en el y elegir "Deploy latest commit".

          Probablemente ahora en el proceso del deploy nos diga que consigió hacer el Build, pero cuando ejecuta "python app.py" aparezca un error gigante. No te desesperes.

          Previamente escribimos "*" para hacer referencia a la última versión de la dependencia en el archivo pipfile. Lo que puede o no ser soportado por Render ( eso lo veremos en el siguiente tipo de error)

O el segundo problema posible:

     2 - La versión de la dependencia no es soportada por render.En este caso render te muestra en su error kilométrico las versiones soportadas (recomiendo elegir la más moderna posible). Se entiende que en este caso la dependnecia está escrita en pipfile pero la version no es soportada por "Render".
     Insisto en que, lo que muestra render con este error es muchísimo texto, anda scroleando hasta ver una lista de versiones. Algo parecido a esto: ( 2.2.4,4.5.2,5.2.3,) Ahi leé el contexto y vas a ver que efectivamente te está informando cuales son las versiones soportadas por Render para esa dependencia.( la última de la lista es la mas moderna siempre)

     Ahora que sabés cual es la versión mas moderna de tu dependencia soportada por Render, vamos al archivo pipfile y modificamos:

          tu_modulo_faltante = "*"
     
     por:

          tu_modulo_faltante = "5.2.3" (Los números son de ejemplo)

     Por último volvemos a ejecutar los pasos 82 y 83 para actualizar pipfile, pipfile.lock y que queden online en nuestro github. Finalmente volvemos a la pagina de "Render" y en la misma vista donde tuvimos el error de deploy, click en el botón "Manual deploy" y elegir nuevamente "Deploy latest commit".

*Es posible que tengas que hacer este proceso de dos pasos por cada una de las dependencias extras que instalaste previo al deploy. Que conste que yo te avisé..*

**88** - En "Render", menú "Logs", si todo salió bien, vas a ver una linea que dice:
     
     "Your servie is live"

En ese caso tenés permitido llorar y gritar a los 4 vientos que tu back-end se encuentra online.

**89** - Te parece que le hagamos un test?

 - a - En la misma página de render vas a ver que a la misma altura del botón "Manual Deploy" pero contra el margen izquierdo tenés el link de tu back-end online ( COPIALO )
 - b - Abrí POSTMAN
 - c - Hacé click en la pestaña "New Request". ( es un "+" al lado de tus otros request)
 - d - Seleccioná el tipo como "POST"
 - e - Rellená la URL con lo que copiaste en la página de render en el paso "a"
 - f - Agregá /admin/users para apuntarle a esa ruta ( que ya deberia estar funcional )
 - g - Abajo de la URL, click en "Body"/"raw" y seleccioná "JSON".
 - h - ingresá un valor de test. Ej:
 ```JSON
 {
    "name":"pepe",
    "email":"test@test.com",
    "password":"12345678"
}
 ```
 - i - Click en "Send"

**90** - Si todo salio bien, POSTMAN te mostrará algo similar a esto como respuesta:
```JSON
{
    "message": "User created successfully."
}
```
Y en los "Logs" de la página de render te va a mostrar también el pedido de POST y el 201 de que todo salió como lo esperado.
Ahora sí. Es momento de tener un flashback de tu vida con una sonrriza de orgullo y decir en voz baja:
"Lo hice"
Anda a respirar aire fresco que se viene el deploy del front.

**91** - Deploy Front-end: Abrimos la carpeta "my-proyect" en VsCode y buscamos el archivo:

     my-proyect\src\js\flux.js


En el, vamos a modificar el fetch de el pedido de registro. O sea, en la función "register"
Actualmente está así:
     
     "http://127.0.0.1:5000/admin/users"

Y nosotros vamos a pegar ahí la url con la que testeamos previamente el back en el punto "89-a":

     "https://  el-nomre-que-le-pusiste .onrender.com/admin/users" ( deberias utilizar tu URL del back-end )

**92** - Una vez cambiada la url vamos a subir los cambios al proyecto online en github, el cual alimenta nuestros deploy:
A esta altura ya lo tendrias que saber de memoria, pero por las dudas...:

     git status
     git add .
     git commit -m "Update the fetch url"
     git push


**93** - Volvemos a https://dashboard.render.com/

     Click en el boton "New+"
     Click en "Static Site"

**94** - Ahora llegó el momento de conectar un proyecto de github. Deberias ver el proyecto que ya tenés actualmente conectado al back-end como una opción para seleccionarlo y conectarlo. Conectalo.

     Click en "Conect"

**95** - Ahora te va a pedir informaciones de configuración:

     Name: ( dale nombre. ej: mi-mejor-proyecto )
     Branch: ( dejalo default en master, si siempre trabajaste en rama master )
     Root Directory: ( no hace falta llenarlo porque esta agarrando el directorio root, o sea la carpeta my-proyect )
     Build Command: yarn build ( ya viene asi default )
     Publish directory: build ( ya viene asi default )

     Click en "Create Static Site" ( va a empezar a deployar )

**96** - Cuando por fin leas "Your site is live" ( Puede demorar unos minutos ), quiere decir que ya podes probar finalmente si tu front-end ya consigue hacer un pedido al back-end y todo corriendo online.
En la misma página donde estas viendo el log " Your site is live"
un poco mas arriba y a la izquierda vas a ver la url de tu Static Site.
Entrá en esa URL.

Si querés hacer que tu back-end solo acepte pedidos de tu front-end, no te olvides de copiar la url recien salida del horno de tu front-end para adjudicarla en el archivo "app.py" .

     En la linea de código del CORS que actualmente está así:

           CORS(app, resources={r"/*": {"origins": "*"}}, supports_credentials=True)
     
     Lo modificás para que sea igual al siguiente ejemplo:

          CORS(app, resources={r"/*": {"origins": "TU_NUEVA_URL_BASE_DE_FRONT_DEPLOYADO"}}, supports_credentials=True)


     *( falta solo 1na cosa a modificar para que quede totalmente funcional.)

Entremos al archivo "flux.js" de nuestro front-end y en nuestro action tendremos que cambiar la url del fetch por la url de nuestro back end renderizado.(acordate de darle el "/admin/user" al final de la url), tené en cuenta que lo que estas reemplazando es la "BASE URL".

POR ÚLTIMO. SUBÍ LOS CAMBIOS COMO SIEMPRE: git status, git add . etc...
Y hacé un Manual deploy nuevo tanto para front como para back :D !

LISTOOO

**97** - Una vez en tu sitio, intentá registrarte poniendo los datos correspondientes:

     Nombre: Ejemplo
     Email: ejemplo@ejemplo.com
     Contraseña: contraseñafalsa123

     y dale click al "Register"

Si todo salió bien vas a leer "Registro Exitoso"

**98** - Asegurate de guardar las 2 url que corresponden tanto a tu back-end, como a tu-front end on-line.
No viene mal que guardes en favoritos la url del dashboard de "Render".

**99** - Entendamos un par de cositas:
- a - Por qué tengo que hacer el deploy en 2 partes si el proyecto es uno?

      Al seleccionar "static site" nos carga predeterminado un ambiente de ejecución "Node" para poder interpretar js.
      Pero si te acordás, el back-end lo tenemos escrito en Python. Por eso es que creamos 2 ambitenes diferentes en Render.

- b - Por qué después de no utilizar el back por un tiempo la primer llamada demora mucho?

      Render, al no detectar actividad durante un tiempo tira el servidor, pero lo levanta automáticamente cuando le hacemos un pedido, este primer pedido después de mucho tiempo de inactividad puede llevar hasta 30 segundos( lo que tarda en volver a estar en linea). No te desesperes, recordá que es un servicio gratuito.

- c - Si el servidor se reinicia después de un tiempo de inactividad, mantiene mis datos de la db?

      Si, el back-end mantiene el archivo database.db creado en su primer ejecución. Esto garantiza que, el código que le pasamos no cree un "database" nuevo pisando el que contiene informaciones cada vez que se reinicia el servidor.
      De esta forma cada vez que el back-end vuelva a estar on-line será utilizando el mismo database de la primer ejecución.
      
- d - Por qué no oculté informaciones como URL, llaves y otras cosas en archivos .env?

      La idea del tutorial es enforcarse al 100 en una estructura muy básica de un proyecto general. Por lo tanto para abarcar tanto terreno se hicieron algunos sacrificios. De cualquier forma, no te desanimes, en breve llegará un tutorial totalmente enfocado en archivos .env y como utilizarlos. Y si andas corto de tiempo, investigando un rato por internet vas a ver que utilizar el .env y deployarlo a Render es algo MUY FACIL.

**100** - Y ya por fin tenés tu Front-End, tu back-end y tu base de datos funcionales y online!
Felicitaciones por llegar hasta acá. Pedimos pizza?

**Esta guia fue creada con mucho cariño por el profesor **David Ezequiel Cunha Quinteros** de 4-Geeks Academy para sus alumnos y compañeros directivos. Cualquier duda pueden escribir a regenerik@gmail.com . Muchísimas gracias.*

<!-- David Ezequiel Cunha Quinteros 26/07/2023 //ultima modif: 17/05/2024 - v17 --> 