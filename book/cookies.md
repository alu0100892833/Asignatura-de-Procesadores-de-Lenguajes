# Cookies
## Descripción
Un cookie es una pequeña información enviada por un sitio web y almacenada en el navegador del usuario, de manera que el sitio web puede consultar la actividad previa del usuario. Los cookies tienen una serie de funciones:

* Tener un control acerca de los usuarios. Cada vez que un usuario introduce su nombre de usuario y su contraseña, se almacena un cookie para que el usuario no tenga que estar identificándose en cada página del servidor.
* Tener información acerca de la información que suele buscar el usuario, por ello existen los problemas de privacidad y es una de las razones por lo que las cookies tiene detractores.
---

## Creación de cookies
Cuando recibimos un petición HTTP, el servidor puede enviar un *Set-Cookie* en la cabecera de la respuesta. Normalmente, esta cookie es almacenada en el navegador y el valor de la cookie es enviado junto a todas las peticiones hechas al mismo servidor. Además, se puede especificar un retraso de caducidad así como restricciones a un dominio y una ruta específicos, limitando el tiempo y el sitio al que se envía la cookie

### The Set-Cookie and Cookie headers
El encabezado de respuesta HTTP Set-Cookie se utiliza para enviar cookies desde el servidor al usuario. Una cookie sencilla se puede configurar de la siguiente manera:
~~~
Set-Cookie: <cookie-name>=<cookie-value>
~~~  

El servidor le dice al cliente que almacene una cookie. La respuesta enviada al navegador contendrá el encabezado *Set-Cookie* y el navegador almacenará la cookie.
~~~
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: cookie_buena=choco
Set-Cookie: cookie_rara=fresita

[page content]
~~~  

Ahora, con cada nueva solicitud al servidor, el navegador enviará todas las cookies previamente almacenadas al servidor usando el encabezado Cookie.
~~~
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: cookie_buena=choco; cookie_rara=fresita
~~~

### Cookies de  sesión

La cookie simple creada anteriormente es una cookie de sesión: Se eliminará cuando el cliente se desconecte dek servidor, duran sólo durante la duración de la sesión. No especifican ninguna directiva de caducidad o de antigüedad máxima. Debemos tener en cuenta que los navegadores web a menudo tienen habilitada la restauración de sesiones, lo que hará que la mayoría de las cookies de sesión sean permanentes, como si el navegador nunca estuviera cerrado.


### Cookies permanentes

En lugar de expirar cuando el cliente está cerrado, las cookies permanentes caducan en una fecha específica (Expires) o después de un período de tiempo específico (Max-Age).
~~~
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
~~~


### Cookies seguras y HttpOnly

Una cookie segura sólo se enviará al servidor cuando se realice una solicitud utilizando SSL y el protocolo HTTPS. Sin embargo, tenga en cuenta que la información confidencial nunca debe almacenarse o transmitirse en cookies HTTP ya que todo el mecanismo es inseguro y este indicador no ofrecerá ningún cifrado o seguridad adicional.


### Alcance de las cookies

Las directivas de dominio y ruta definen el ámbito de la cookie, es decir, el conjunto de URL a las que se deben enviar las cookies.

**El dominio** especifica los hosts a los que se enviará la cookie. Si no se especifica, el valor predeterminado es la parte del host de la ubicación del documento actual (pero sin incluir subdominios). Si se especifica un dominio, los subdominios siempre se incluyen.

Si se establece Domain = ull.org, las cookies se incluyen en subdominios como etsii.ull.org.

**Path** indica una ruta de acceso de URL que debe existir en el recurso solicitado antes de enviar el encabezado de Cookie. El carácter %x2F("/") se interpreta como un separador de directorios y los subdirectorios se emparejarán también.

Si se establece Path =/ docs, todos estos caminos coincidirán:

* "/Docs",
* "/Docs/Web/",
* "/Docs/Web/HTTP"
---
## Seguridad

### Secuestro de sesión y XSS

Las cookies se utilizan a menudo en la aplicación web para identificar un usuario y su sesión autenticada. Así que robar una cookie de una aplicación web puede llevar a secuestrar la sesión del usuario autenticado. Las maneras comunes de robar cookies incluyen el uso de la ingeniería social o explotando una vulnerabilidad XSS en la aplicación.


El atributo HttpOnly cookie puede ayudar a mitigar este ataque al impedir el acceso al valor de cookie a través de JavaScript.

### Falsificación de peticiones entre sitios (CSRF)

Hay veces que por ejemplo alguien incluye una imagen que no es realmente una imagen (por ejemplo, en un foro o foro no filtrado), en su lugar es realmente una solicitud al servidor de su banco para retirar dinero:
~~~
<Img src = "http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory">
~~~
Ahora, si ha iniciado sesión en su cuenta bancaria y sus cookies siguen siendo válidas (y no hay ninguna otra validación), transferirá dinero tan pronto como cargue el HTML que contiene esta imagen. Hay algunas técnicas que se utilizan para evitar que esto suceda:
* Al igual que con XSS, el filtrado de entrada es importante.
* Siempre debe haber una confirmación requerida para cualquier acción sensible.
* Las cookies que se utilizan para acciones sensibles deben tener una vida útil corta.
* Para obtener más consejos de prevención, consulte la hoja de trucos de prevención [OWASP CSRF](https://www.owasp.org/).

NOTA: La información confidencial o confidencial nunca debe ser almacenada o transmitida en cookies HTTP ya que el mecanismo completo es intrínsecamente inseguro.


## Seguimiento y privacidad

#### Cookies de terceros

Las cookies tienen un dominio asociado a ellas. Si estas en el mismo dominio que una determinada cookie, se dice qeu es una cookie de primera persona.Sin embargo, si el dominio es diferente, la cookie se referencia como una **cookie de terceros**. Las cookies de primera persona son las que se envían al servidor del dominio en el que estamos. Pero una página web, puede contener imágenes o elementos almacenados en servidores de otros dominios como un anuncio o banner. Las cookies que se envían a esos servidores externos son las llamadas cookies de terceros, y se usan principalmente para publicidad. La mayoría de los navegadores **permiten** las cookies de terceros por defecto, pero existen complementos para bloquearlas.

#### Do-Not-Track

No hay requisitos legales para su uso, pero la cabecera HTTP Do-Not-Track p
uede usarse para indicarle a una aplicación web que debe desactivar el paso
 de nuestra a información a terceros

#### Directiva sobre cookies de la Unión Europea

En la directiva de la UE, se especifica que antes que guarde o distribuya información de un ordenador, móvil u otro dispositivo, el usuario debe **dar su consentimiento**. Desde entonces casi todas las páginas web informan al
 usuario sobre el uso de cookies.

#### Zombie cookies y "Evercookies"

Un uso más radical de las cookies son las **zombie cookies** o las **"Evercookies"** (Cookies para siempre). Éstas, son un tipo de cookie que se recre
a de nuevo, una vez se ha eliminado y además se crean intencionadamente para que sean difíciles de borrar para siempre.

