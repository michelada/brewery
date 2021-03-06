# Renderizado de tiempo en Rails

### Formatos de tiempo localizables

Es muy común encontrar aplicaciones de rails en donde se hace uso de [strftime](https://ruby-doc.org/core-3.0.2/Time.html#method-i-strftime) para el formato de fechas. Y si bien estas implementaciones funcionan, tienen una pequeña característica
"hard codean" el tiempo a un formato especifico. Y terminan duplicando el código, lo cual puede generar inconsistencias en los formatos utilizados en la aplicación. Aunado a esto es que impide la localización del tiempo en diferentes idiomas.

En rails tenemos métodos de ayuda para internacionalización que nos permiten la localización de manera rápida, sencilla y sin tener que recurrir a la documentación de `strftime`.

```ruby
  I18n.l(Time.now)
```

o bien desde las vistas simplemente podemos llamar

```erb
  <!-- app/views/home/index.html.erb -->
  <p><%= l Time.now, format: :short %></p>
```

Este método de [localización](https://guides.rubyonrails.org/i18n.html#adding-date-time-formats) utiliza los archivos de configuraciones de i18n ubicadas en `config/locales/*.yml`. Habilitando la capacidad de internacionalización (traducciones) en objetos Date, Time y DateTime.

## Distancia de tiempo en palabras

Por otro lado el popular método `distance_of_time_in_words` hace que nuestros objetos de tiempo se dibujen a formatos humanamente legibles (ej. "hace 5 meses", "hace 3 minutos", "dentro 3 horas").

```ruby
  from_time = Time.now
  distance_of_time_in_words(from_time, from_time + 50.minutes)
  # => about 1 hour
```

Esto presenta un problema, dado que la zona horaria de nuestro servidor no siempre es igual a la de los clientes, por lo que entonces se tiene que hacer el ajuste, por request. Un calculo innecesario que nuestro servidor puede omitir al delegar esta tarea al lado del cliente, por medio de javascript. En los cuales tenemos varias opciones

### local-time.js

La primera y mas sencilla seria por medio del [local-time](https://www.npmjs.com/package/local-time) creado por besacamp. Solo require que lo inicialicemos en `application.js`

**js**
```js
// app/javascript/application.js
import LocalTime from "local-time"
LocalTime.start()
```

Y con un time tag podemos hacer esto

**html**
```erb
<!--  app/views/home/index.html.erb -->
<%= time_tag article.published_at, "data-local": "time-ago" %>
```

### date-fsn + stimulus

Otra opción seria utilizando uno de los paquetes mas completos de manejo de tiempo en js [date-fns](https://date-fns.org) y con la ayuda de un controller de [stimulus](https://stimulus.hotwired.dev) tendríamos un resultado similar con algo como esto:

**js**
```js
// app/javascript/controllers/time_controller.js
import { Controller } from "@hotwired/stimulus"
import { parseISO, formatDistanceToNow } from 'date-fns'

export default class extends Controller {
  static values = { timestamp: String }

  connect() {
    this.element.textContent = formatDistanceToNow(this.time, { addSuffix: true })
  }

  get time() {
    return parseISO(this.timestampValue);
  }
}
```

**html**
```html
  <!--  app/views/home/index.html.erb -->
  <div data-controller="time" data-time-timestamp-value="<%= article.published_at.iso8601 %>"></div>
```

Al hacer esto evitamos que el servidor realize varias ciclos de procesamiento por cada vez que se encuentra con un objeto de tiempo.

Si hacemos una pequeña prueba en donde se renderizen 250 fechas de tiempo como podemos ver en el [ejemplo](https://github.com/3zcurdia/teacher/blob/render-time/app/views/application/index.html.erb) tenemos una respuesta promedio.

```
Rendered application/index.html.erb within layouts/application (Duration: 36.7ms | Allocations: 11974)
Rendered layout layouts/application.html.erb (Duration: 41.3ms | Allocations: 15054)
Completed 200 OK in 44ms (Views: 40.7ms | ActiveRecord: 1.6ms | Allocations: 15400)
```

Mientras que utilizando `distance_of_time_in_words`

```
Rendered application/index.html.erb within layouts/application (Duration: 39.0ms | Allocations: 29871)
Rendered layout layouts/application.html.erb (Duration: 42.6ms | Allocations: 32871)
Completed 200 OK in 44ms (Views: 41.9ms | ActiveRecord: 1.2ms | Allocations: 33186)
```

Lo que podemos destacar de la implementación de javascript, más allá de que el tiempo de respuesta es menor. Es que **las asignaciones totales decrecen en un 50%** ya que solo usamos un formato estándar `iso8601`. Mientras que la implementación de `distance_of_time_in_words` utiliza más código y por ende realiza más ubicaciones en memoria.
