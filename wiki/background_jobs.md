# Background Jobs

Los background jobs son procesos que corren en segundo plano y sin intervención del usuario.

Algunos ejemplos de tareas que se pueden realizar en segundo plano son:

- Enviar correos
- Enviar notificaciones
- Ejecutar transacciones
- Generar documentos PDFs
- Hacer un procesado de datos mediante scrappers o apis

## Por qué usar background jobs?

Los background jobs nos ayudan a mejorar el tiempo de respuesta de nuestra aplicación ya que permite extraer tareas que consumen mucho tiempo y ejecutarlas de manera asíncrona.

De esta manera, al mandar por ejemplo un form de registro, el usuario verá la pantalla de éxito inmediatamente después de llenar el form mientras que algunos procesos como enviar un email de confirmación o recomendar amigos corren en segundo plano.

![Image of a background job process]
(https://images.viblo.asia/19911b2e-21bc-4402-9b99-8f8e8b70e5d5.jpg)

# Active Job

Active Job es un framework para declarar procesos.
Funciona principalmente para declarar "Jobs" en nuestro proyecto, encargados de ejecutarse en segundo plano, su estructura principal se basa en una cola de procesos programados que son ejecutados a medida de su prioridad.

La función principal dentro de Active Job es que se pueda ejecutar cualquier actividad en pequeñas unidades de trabajo, y correr de manera asíncrona con el proyecto. Para ello ActiveJob requiere de un queuing backend para ejecutar los procesos de segundo plano. Algunos de los queuing backends que soporta Active Job son: Sidekiq, Resque, DelayedJob o Sucker Punch, siendo Sidekiq el mas utilizado.

[ActiveJob Basics](https://guides.rubyonrails.org/active_job_basics.html)

# Sidekiq

Sidekiq es un open-source framework que provee un procesamiento en segundo plano eficiente para aplicaciones de Ruby. Usa Redis como una fuente de datos para almacenar todos los jobs y datos operacionales.

Provee las capacidad para ejecutar cualquier proceso asíncronamente o ser calendarizado para ejecutarse más tarde al mandarlos al final de la cola. También ofrece estrategias para repetir los Jobs en caso de que la ejecución falle.

[Sidekiq getting started](https://github.com/mperham/sidekiq/wiki/Getting-Started)
