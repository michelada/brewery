# Protocolo HTTP

## *Definición*

HTTP, de sus siglas en inglés: "Hypertext Transfer Protocol", es el protocolo en el que se basa la Web. Fue inventado por Tim Berners-Lee entre los años 1989-1991. HTTP ha visto muchos cambios, manteniendo la mayor parte de su simplicidad y desarrollando su flexibilidad. Es la base de cualquier intercambio de datos en la Web, y un protocolo de estructura cliente-servidor, esto quiere decir que una petición de datos es iniciada por el elemento que recibirá los datos (el cliente), normalmente un navegador Web.

- - -

## *Versiones*

* #### HTTP/1.0

    * Se agrega la versión del protocolo cuando se envía una petición, por ejemplo: HTTP/1.0.
    * Se envía un código de estado al comienzo de la respuesta, permitiendo así que el navegador pueda responder al éxito o fracaso de la petición realizada.
    * El concepto de cabeceras de HTTP, se presentó tanto para las peticiones como para las respuestas, permitiendo la trasmisión de meta-data.
    * Con el uso de las cabeceras de HTTP, se pudieron transmitir otros documentos además de HTML, mediante la cabecera _Content-Type_.
    * Hay una conexión TCP por cada petición/respuesta intercambiada, presentando estos dos grandes inconvenientes: abrir y crear una conexión requiere varias rondas de mensajes, por lo tanto resultaba lento.

* #### HTTP/1.1

    * Una conexión podía ser reutilizada, ahorrando así el tiempo de re-abrirla repetidas veces para mostrar los recursos empotrados dentro del documento original pedido.
    * Enrutamiento ('Pipelining') se añadió a la especificación, permitiendo realizar una segunda petición de datos, antes de que fuera respondida la primera, disminuyendo de este modo la latencia de la comunicación: cabecera _Connection_ con el valor _keep-alive_.
    * Se permitió que las respuestas a peticiones, podían ser divididas en sub-partes.
    * Se añadieron controles adicionales a los mecanismos de gestión de la cache.
    * La negociación de contenido, incluyendo el lenguaje, el tipo de codificación o tipos, se añadieron a la especificación, permitiendo que servidor y cliente, acordaran el contenido más adecuado a intercambiarse.
    * Gracias a la cabecera _Host_, pudo ser posible alojar varios dominios en la misma dirección IP.


* #### HTTP/2.0

    * Es un protocolo binario, en contraposición a estar formado por cadenas de texto, tal y como estaban basados las versiones anteriores. Así no se puede leer directamente, ni crear manualmente.
    * Es un protocolo multiplexado. Peticiones paralelas pueden hacerse sobre la misma connexión, no está sujeto pues a mantener el orden de los mensajes, ni otras restricciones que tenían los protocolos anteriores HTTP/1.x
    * Comprime las cabeceras, ya que estas, normalmente son similares en un grupo de peticiones. Esto elimina la duplicación y retardo en los datos a transmitir.
    * Esto permite al servidor almacenar datos en la caché del cliente, previamente a que estos sean pedidos, mediante un mecanismo denominado 'server push'.

- - -

## *Componentes de una URL*

Una URL para HTTP  normalmente consta de tres o cuatro componentes:

### Scheme

El esquema identifica el protocolo que se utilizará para acceder al recurso en Internet. Puede ser HTTP (sin SSL) o HTTPS (con SSL).

### Host

Identifica el host que contiene el recurso. Por ejemplo:www.ejemplo.com. Un servidor proporciona servicios en nombre del host, pero los hosts y servidores no tienen una asignación uno a uno. Los nombres de host también pueden ir seguidos de un número de puerto. Los números de puerto conocidos para un servicio normalmente se omiten de la URL. La mayoría de los servidores utilizan los números de puerto conocidos para HTTP y HTTPS, por lo que la mayoría de las URL de HTTP omiten el número de puerto.

### Path

La ruta identifica el recurso específico en el host al que el cliente web quiere acceder. Por ejemplo: /software/htp/cics/index.html.

### Query String

Si se usa una cadena de consulta, sigue el componente de ruta y proporciona una cadena de información que el recurso puede usar para algún propósito (por ejemplo, como parámetros para una búsqueda o como datos para procesar). La cadena de consulta suele ser una cadena de pares de nombre y valor; por ejemplo: term=bluebird. Los pares de nombre y valor están separados entre sí por un ampersand (&); por ejemplo: term=bluebird&source=browser-search.

El esquema y los componentes de host de una URL no se definen como mayúsculas y minúsculas, pero la ruta y la cadena de consulta son sensibles a mayúsculas y minúsculas. Por lo general, la URL completa se especifica en minúsculas.

Los componentes de la URL se combinan y delimitan de la siguiente manera:

`scheme://host:port/path?query`

- El esquema es seguido por dos puntos y dos barras diagonales.
- Si se especifica un número de puerto, ese número sigue al nombre del host, separado por dos puntos.
- El nombre de la ruta comienza con una sola barra diagonal.
- Si se especifica una cadena de consulta, está precedida por un signo de interrogación.`

- - -

## *Componentes de HTTP*

* #### Métodos

HTTP define 8 métodos (algunas veces referido como "verbos") que indica la acción que desea que se efectúe sobre el recurso identificado. Lo que este recurso representa, si los datos pre-existentes o datos que se generan de forma dinámica, depende de la aplicación del servidor. A menudo, el recurso corresponde a un archivo o la salida de un ejecutable que se encuentra en el servidor.


#### GET

Pide una representación del recurso especificado. Por seguridad no debería ser usado por aplicaciones que causen efectos ya que transmite información a través de la URI agregando parámetros a la URL.

_Ejemplo:_

    GET /images/logo.png HTTP/1.1

Explicación: obtiene un recurso llamado logo.png

_Ejemplo con parámetros:_

    GET /index.php?page=main&lang=es

#### POST

Aunque se puedan enviar datos a través del método GET, en muchos casos se utiliza POST por las limitaciones de GET. El contenido va en el body del request, no aparece nada en la URL, aunque se envía en el mismo formato que con el método GET. Si se quiere enviar texto largo o cualquier tipo de archivo este es el método apropiado.

#### HEAD

Pide una respuesta idéntica a la que correspondería a una petición GET, pero sin el cuerpo de la respuesta. Esto es útil para la recuperación de meta-información escrita en los encabezados de respuesta, sin tener que transportar todo el contenido.

#### PUT

Sube, carga o realiza un upload de un recurso especificado (archivo), es el camino más eficiente para subir archivos a un servidor, esto es porque en POST utiliza un mensaje multipart y el mensaje es decodificado por el servidor. En contraste, el método PUT te permite escribir un archivo en una conexión socket establecida con el servidor.
La desventaja del método PUT es que los servidores de hosting compartido no lo tienen habilitado.

_Ejemplo:

    PUT /path/filename.html HTTP/1.1

#### DELETE

Este método le solicita al servidor web que se borre un recurso en específico.

#### TRACE

Este método Permite monitorear los mensajes que hay entre el cliente y el servidor web. Principalmente se usa con propósitos de diagnósticos de fallas o para revisar si existen servidores intermediarios en la conexión.

#### OPTIONS

Devuelve los métodos HTTP que el servidor soporta para un URL específico.Esto puede ser utilizado para comprobar la funcionalidad de un servidor web mediante petición en lugar de un recurso específico.

#### CONNECT

Este método se utiliza para solicitar una conexión de tipo túnel TCP/IP. Principalmente se utiliza cuando se necesita utilizar un proxy para una conexión segura cifrada HTTPS o para comunicaciones vía SSL.

* #### Headers

Las cabeceras (headers) HTTP permiten al cliente y al servidor enviar información adicional junto a una petición o respuesta. Una cabecera de petición esta compuesta por su nombre (no sensible a las mayúsculas) seguido de dos puntos ':', y a continuación su valor (sin saltos de línea). Los espacios en blanco a la izquierda del valor son ignorados
Se pueden agregar cabeceras propias personalizadas usando el prefijo 'X-', pero esta convención se encuentra desfasada desde Julio de 2012, debido a los inconvenientes causados cuando se estandarizaron campos no estándar en el RFC 6648; otras están listadas en un registro IANA, cuyo contenido original fue definido en el RFC 4229, IANA tambien mantiene un registro de propuestas para nuevas cabeceras HTTP.

Las Cabeceras pueden ser agrupadas de acuerdo a sus contextos:

___Cabecera general:___ Cabeceras que se aplican tanto a las peticiones como a las respuestas, pero sin relación con los datos que finalmente se transmiten en el cuerpo.
___Cabecera de consulta:___ Cabeceras que contienen más información sobre el contenido que va a obtenerse o sobre el cliente.
___Cabecera de respuesta:___ Cabeceras que contienen más información sobre el contenido, como su origen o el servidor (nombre, versión, etc.).
___Cabecera de entidad:___ Cabeceras que contienen más información sobre el cuerpo de la entidad, como el tamaño del contenido o su tipo MIME (son una serie de convenciones o especificaciones dirigidas al intercambio a través de Internet de todo tipo de archivos de forma transparente para el usuario).

##### Cabeceras comunes para peticiones y respuestas:

* ___Content-Type:___ descripción MIME de la información contenida en este mensaje. Es la referencia que utilizan las aplicaciones Web para dar el correcto tratamiento a los datos que reciben.
* ___Content-Length:___ longitud en bytes de los datos enviados, expresado en base decimal.
* ___Content-Encoding:___ formato de codificación de los datos enviados en este mensaje. Sirve, por ejemplo, para enviar datos comprimidos (x-gzip o x-compress) o encriptados.
* ___Date:___ fecha local de la operación. Las fechas deben incluir la zona horaria en que reside el sistema que genera la operación. Por ejemplo: Sunday, 12-Dec-96 12:21:22 GMT+01. No existe un formato único en las fechas; incluso es posible encontrar casos en los que no se dispone de la zona horaria correspondiente, con los problemas de sincronización que esto produce. Los formatos de fecha a emplear están recogidos en los RFC 1036 y 1123.
* ___Pragma:___ permite incluir información variada relacionada con el protocolo HTTP en el requerimiento o respuesta que se está realizando. Por ejemplo, un cliente envía un Pragma: no-cache para informar de que desea una copia nueva del recurso especificado.

##### Cabeceras sólo para peticiones del cliente

* ___Accept:___ campo opcional que contiene una lista de tipos MIME aceptados por el cliente. Se pueden utilizar * para indicar rangos de tipos de datos; tipo/* indica todos los subtipos de un determinado medio, mientras que */* representa a cualquier tipo de dato disponible.
* ___Authorization:___ clave de acceso que envía un cliente para acceder a un recurso de uso protegido o limitado. La información incluye el formato de autorización empleado, seguido de la clave de acceso propiamente dicha. La explicación se incluye más adelante.
* ___From:___ campo opcional que contiene la dirección de correo electrónico del usuario del cliente Web que realiza el acceso.
* ___If-Modified-Since:___ permite realizar operaciones GET condicionales, en función de si la fecha de modificación del objeto requerido es anterior o posterior a la fecha proporcionada. Puede ser utilizada por los sistemas de almacenamiento temporal de páginas. Es equivalente a realizar un HEAD seguido de un GET normal.
* ___Referer:___ contiene la URL del documento desde donde se ha activado este enlace. De esta forma, un servidor puede informar al creador de ese documento de cambios o actualizaciones en los enlaces que contiene. No todos los clientes lo envían.
* ___User-agent:___ cadena que identifica el tipo y versión del cliente que realiza la petición. Por ejemplo, los browsers de Netscape envían cadenas del tipo User-Agent: Mozilla/3.0 (WinNT; I)

##### Cabeceras sólo para respuestas del servidor HTTP

* ___Allow:___ informa de los comandos HTTP opcionales que se pueden aplicar sobre el objeto al que se refiere esta respuesta. Por ejemplo, Allow: GET, POST.
* ___Expires:___ fecha de expiración del objeto enviado. Los sistemas de cache deben descartar las posibles copias del objeto pasada esta fecha. Por ejemplo, Expires: Thu, 12 Jan 97 00:00:00 GMT+1. No todos los sistemas lo envían. Puede cambiarse utilizando un \<META EXPIRES\> en el encabezado de cada documento.
* ___Last-modified:___ fecha local de modificación del objeto devuelto. Se puede corresponder con la fecha de modificación de un fichero en disco, o, para información generada dinámicamente desde una base de datos, con la fecha de modificación del registro de datos correspondiente.
* ___Location:___ informa sobre la dirección exacta del recurso al que se ha accedido. Cuando el servidor proporciona un código de respuesta de la serie 3xx, este parámetro contiene la URL necesaria para accesos posteriores a este recurso.
* ___Server:___ cadena que identifica el tipo y versión del servidor HTTP. Por ejemplo, Server: NCSA 1.4.
* ___WWW-Autenticate:___ cuando se accede a un recurso protegido o de acceso restringido, el servidor devuelve un código de estado 401, y utiliza este campo para informar de los modelos de autentificación válidos para acceder a este recurso.

* #### Códigos y mensajes de respuesta

La lista de los códigos de estado de HTTP se divide en 5 categorías:

* ___100’s:___ Códigos informativos indicando que la solicitud iniciada por el navegador es constante.

* ___200’s:___ Códigos exitosos devueltos cuando la petición del explorador se ha recibido correctamente, entendido, y procesado por el servidor.

* ___300’s:___ Los códigos de redirección se devuelven cuando un nuevo recurso ha sido sustituido por el recurso solicitado.

* ___400’s:___ Códigos de error del cliente indicando que existe un problema con la solicitud.

* ___500’s:___ Códigos de error del servidor indicando que la petición fue aceptada, pero que un error en el servidor impidió el cumplimiento de la solicitud.

## *Mensajes HTTP*

#### Requests

![HTTP_REQUEST](https://media.prod.mdn.mozit.cloud/attachments/2016/08/09/13687/5d4c4719f4099d5342a5093bdf4a8843/HTTP_Request.png)

La primera línea es la request line y el resto son cabeceras HTTP.
Una petición de HTTP, está formado  por los siguientes campos:

* Un método HTTP. El objetivo de un cliente, suele ser una petición de recursos, usando GET, o presentar un valor de un formulario HTML, usando POST, aunque en otras ocasiones puede hacer otros tipos de peticiones.
* La dirección del recurso pedido (Path).
* Cabeceras HTTP opcionales, que pueden aportar información adicional a los servidores.
* Opcional, un cuerpo de mensaje en algún método, como puede ser POST, en el cual envía la información para el servidor.

#### Responses

![HTTP_RESPONSE](https://media.prod.mdn.mozit.cloud/attachments/2016/08/09/13691/58390536967466a1a59ba98d06f43433/HTTP_Response.png)

La primera línea es la status line o línea de estado, seguida de las cabeceras HTTP y finalmente el contenido servido.
Las respuestas están formadas por los siguentes campos:

* La versión del protocolo HTTP que están usando.
* Un código de estado, indicando si la petición ha sido exitosa, o no, y debido a que.
* Un mensaje de estado, una breve descripción del código de estado.
* Cabeceras HTTP, como las de las peticiones.
* Opcionalmente, el recurso que se ha pedido.


## Fuentes

* [Componentes de URL](https://www.ibm.com/support/knowledgecenter/SSGMCP_5.1.0/com.ibm.cics.ts.internet.doc/topics/dfhtl_uricomp.html)
* [Versiones de HTTP](https://somostechies.com/que-es-http2/)
* [Versiones de HTTP (2)](https://developer.mozilla.org/es/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)
* [Métodos de HTTP](https://sites.google.com/site/httpsprotocolo/versiones-de-http)
* [Métodos de HTTP (2)](https://es.wikibooks.org/wiki/M%C3%A9todos_HTTP)
* [Mensajes HTTP](https://developer.mozilla.org/es/docs/Web/HTTP/Overview)
* [Códigos de respuesta](https://developer.mozilla.org/es/docs/Web/HTTP/Status)
* [Códigos de respuesta (2)](https://kinsta.com/es/blog/codigos-de-estado-de-http/)
* [Headers](https://developer.mozilla.org/es/docs/Web/HTTP/Headers)
* [Headers (2)](http://redesdecomputadores.umh.es/aplicacion/HTTP.htm)
