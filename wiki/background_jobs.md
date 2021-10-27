# Background Jobs

Los background jobs son procesos que corren en segundo plano y sin intervención del usuario.

Algunos ejemplos de tareas que se pueden realizar en segundo plano son:

- Enviar emails
- Mandar notificaciones
- Ejecutar transaccciones
- Generar PDFs

## Por qué usar background jobs?

Los background jobs nos ayudan a mejorar el tiempo de respuesta de nuestra applicación ya que permite extraer tareas que consumen mucho tiempo y ejecutarlas de manera asíncrona.

De esta manera, al mandar por ejemplo un form de registro, el usuario verá la pantalla de éxito inmediatamente después de llenar el form mientras que algunos procesos como enviar un email de confirmación o recomendar amigos corren en segundo plano.

![Image of a background job process]
(https://images.viblo.asia/19911b2e-21bc-4402-9b99-8f8e8b70e5d5.jpg)

# Active Job

Active Job es un framework para declarar procesos.
Funciona principalmente para declarar "Jobs" en nuestro proyecto, encargados de ejecutarse en segundo plano, su estructura principal se basa en una cola de procesos programados que son ejecutados a medida de su prioridad.

La función principal dentro de Active Job es que se pueda ejecutar cualquier actividad en pequeñas unidades de trabajo, y correr de manera asíncrona con el proyecto.

Un dato importante al comenzar a trabajar con Active Job es que de manera predeterminada, nuestro proyecto cuenta con un método de ejecución asíncrono que permite la ejecución de Jobs en segundo plano, pero se encarga unicamente de almacenarlos en la memoria RAM, de tal manera que si el proyecto se reinicia, los procesos que están en la cola se pierden.

Por esa razón, para un proyecto formal, se requiere el uso de un queuing backend.

Algunos de los queuing backends que soporta Active Job son:

- Sidekiq
- Resque
- DelayedJob
- Backburner
- Que
- queue_classic
- Sneakers
- Sucker Punch

[Más información](https://guides.rubyonrails.org/active_job_basics.html)

# Sidekiq

Sidekiq es un open-source framework que provee un procesamiento en segundo plano efficiente para aplicacciones de Ruby. Usa Redis como una estructura de datos en memoria para almacenar todos sus jobs y datos operacionales.

Provee las caracteristicas para ejecutar cualquier proceso asíncronamente o ser calendarizado para ejecutarse más tarde al mandarlos al final de la cola. También ofrece estrategias para repetir los Jobs en caso de que la ejecución falle.

Sidekiq requiere de 3 componentes o partes para funcionar:

- Cliente: Quien crea los procesos (Jobs)
- Redis: La estructura de almacenamiento para los procesos (Queue)
- Servidor: Quien se encarga de sacar los procesos de la cola y ejecutarlos

[Sidekiq docs](https://github.com/mperham/sidekiq/wiki/Getting-Started)
