# Sessions en ExpressJS
Existen dos formas generales de implementar sesiones en Express: utilizar cookies y utilizar un almacén de sesiones en el backend. Ambos añaden un nuevo objeto en el objeto request denominado session, que contiene las variables de sesión.

No importa el método que utilice, Express proporciona una interfaz coherente para trabajar con los datos de la sesión.

## Sesión basada en cookies

Utilizando el hecho de que las cookies pueden almacenar datos en el navegador de los usuarios, se puede implementar una API de sesiones mediante cookies. Express viene con un middleware integrado llamado cookieSession, que hace precisamente eso.

Cargue el middleware cookieParser con un secreto, seguido por el middleware cookieSession, antes del middleware del enrutador. El middleware cookieSession depende del cookieParser middlware porque utiliza una cookie para almacenar los datos de la sesión.

El middleware cookieParser debe inicializarse con un secreto, ya que cookieSession necesita generar una cookie HttpOnly firmada para almacenar los datos de la sesión. Si no especifica un secreto para cookieParser, deberá especificar la opción secreta de cookieSsession.

El siguiente es el código para habilitar sesiones en Express utilizando el middleware cookieSession.
~~~
app.use(express.cookieParser('S3CRE7'));
app.use(express.cookieSession());
app.use(app.router);
~~~

Aunque el middleware cookieSession se puede inicializar sin opciones, el middleware acepta un objeto de opciones con las siguientes opciones posibles:

* Key: Nombre de la cookie. Por defecto connect.sess.
* secret: contraseña para firmar la sesión. Es requerido si CookieParser no se inicializa con uno.
* cookie: Configuración de la cookie de sesión. Se aplican los valores predeterminados de las cookies.
* proxy: Para confiar en el proxy inverso o no. El valor predeterminado es false.

A continuación un ejemplo:
~~~
app.use(express.cookieSession({
  key: 'app.sess',
  secret: 'SUPERsekret'
}));
~~~


## Sesión basada en Session-store

Una Session-store es una provisión para almacenar datos de sesión en el backend. Las sesiones basadas en almacenes de sesiones pueden almacenar una gran cantidad de datos que están bien ocultos al usuario.

El middleware de sesión proporciona una forma de crear sesiones utilizando los almacenes de sesiones. Al igual que cookieSession, el middleware de sesión depende del middleware cookieParser para crear una cookie HttpOnly firmada.

Inicializar la sesión middlware es mucho como inicializar cookieSession - primero cargar cookieParser con un secreto, y cargar el middleware de la sesión, antes de que el middleware del enrutador.
~~~
app.use(express.cookieParser('S3CRE7'));
app.use(express.session());
app.use(app.router);
~~~

El middleware de sesión acepta un objeto de opciones que se puede utilizar para definir las opciones del middleware. Las siguientes son las opciones compatibles:

* Key: Nombre de la cookie. Por defecto connect.sess.
* secret: contraseña para firmar la sesión. Es requerido si CookieParser no se inicializa con uno.
* cookie: Configuración de la cookie de sesión. Se aplican los valores predeterminados de las cookies.
* proxy: Para confiar en el proxy inverso o no. El valor predeterminado es false.

A continuación un ejemplo:
~~~
app.use(express.session({
  key: 'app.sess',
  store: new RedisStore,
  secret: 'SEKR37'
}));
~~~

