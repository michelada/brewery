# RabbitMQ

Es un message queue broker facil y sencillo. Inicialmente fue desarrollado sobre el protocolo programable [AMQP](https://www.rabbitmq.com/tutorials/amqp-concepts.html), y posteriormente se le agrego soporte para [multiples protocolos](https://www.rabbitmq.com/protocols.html)

## Componentes

El modelo de mensajes RabbitMQ se compone de:

- **Producer**: Un productor es una aplicación que envía mensajes.
- **Exchange**: Recibe mensajes de los productores y los envia a los queues
- **Queue**: Es un búfer que almacena mensajes.
- **Consumer**: Es una aplicación de usuario que recibe mensajes.

![rabbitmq](assets/rabbitmq.png)

## Tipos de  Exchange

Dentro de rabbitmq el exchange determina el flujo de los mensajes, cumple la función de router para los distintos queues.

- **fanout**: funciona a manera de broadcast y envia los mensajes a todos los queues enlazados
- **direct**: envia los mensajes que coincidan con la llave de ruta dada (ej. `news.mexico`)
- **topic**: envia los mensajes que hagan match parcial con la ruta (ej. `news.*`)
- **headers**: a manera de topics pero utiliza los headers del mensaje
