# Hotwire

Hotwire nos ayuda a crear aplicaciones web modernas mediante el envío de HTML por cable. Esto permite que las páginas se carguen por primera vez rápidamente, haciendo reutilización de templates de lado del servidor, permitiendo una experiencia de desarrollo más simple y productiva en cualquier lenguaje de programación, sin sacrificar la velocidad o capacidad de respuesta de una Single Page Application.

Retomando el tema del HTML generado por el servidor, Ruby on Rails es un framework web del lado del servidor y junto con Hotwire nos permite realizar llamadas asíncronas. Ya no tendremos la necesidad de agregar un framework de js para el lado del cliente, permitiendonos reutilizar todo el HTML generado desde el servidor.

Hotwire se compone de 3 principales librerías que son:

- Turbo
- Stimulus
- Strada

## Turbo

Nos ofrece la velocidad de  Single Page Application sin tener que escribir JavaScript. Turbo acelera los enlaces y los envíos de formularios sin necesidad de cambiar el HTML generado por el servidor.
Turbo Navega dentro de un proceso persistente y en segundo plano, lo que quiere decir es que podemos en un proceso llamar el contenido que queremos mostrar con un href sin tener que recargar la página completa.

[más detalles]({{ site.baseurl }}/turbo)

## Stimulus

JS framework simple. Stimulus está diseñado para mejorar HTML estático o renderizado por servidor.
Stimulus nos permite agregar funciones JS personalizadas. Turbo se enfoca directamente en actualizar el DOM y luego asume que conectará cualquier comportamiento adicional usando acciones con stimulus.


## Strada

Estandariza la forma en que la web y las partes nativas de una aplicación híbrida móvil se comunican entre sí a través de atributos de puente HTML.

De momento la [página oficial](https://hotwired.dev) aun no tiene fecha exacta de liberacion.
