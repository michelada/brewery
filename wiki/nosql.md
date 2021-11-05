# NoSQL

Son aquellas bases de datos que **no utilizan el modelo relacional como paradigma principal** para el manejo de datos.

El termino NoSQL fue acuñado a finales de la decada de los 2000's. Sin embargo desde los 60's se almacenan datos, y es hasta finales de los 70's cuando surge el concepto de SQL con el paper de Edgar Codd, "A Relational Model of Data for Large Shared Data Banks". Existen cuatro tipos de NoSQL: *Key-Value*, *Document*, *Wide Column* y *Graph*,

## Key-Value

![key-value](assets/db-key-value.png)

Cada elemento de datos en la base de datos se almacena como un par llave-valor. Algo muy similar a un *hashmap*, pero persistente.

### Casos de uso

- Cache
- Busquedas simples
- Datos transaccionales
- Datos de acceso rápido
- QueueJobs

### Implementaciones

- [Redis](https://redis.io)
- [DynamoDB](https://aws.amazon.com/dynamodb/)

### Alternativas
- [ETS(Erlang)](https://beta.erlang.org/docs/19/man/ets.html): Dentro del ecosistema de Erlang/Elixir, la OTP provee de una alternativa a bases de datos key-base.
- [RabitMQ](https://www.rabbitmq.com): Si la arquitectura a implementar es de pipelines o queues

## Document

![document-based](assets/db-document.png)

Mantiene una estructura de datos similar a un formato tipo *JSON* en donde permite anidar documentos.

### Casos de uso

- Propósito general
- Estructuras de datos dinámicas

### Implementaciones

- [MongoDB](https://www.mongodb.com)
- [Firebase](https://firebase.google.com)

### Alternativas
- [JSON(PostgresSQL)](https://www.postgresql.org/docs/current/datatype-json.html): Dentro PostgresSQL se pueden utilizar columnas de tipo JSON y JSONB. Las cuales habilitan la misma flexibilidad.
- [MNesia(Erlang)](http://erlang.org/doc/man/mnesia.html): Dentro del ecosistema de Erlang/Elixir, la OTP provee de una alternativa a bases de datos distribuidas.

## Wide-Column

![wide-column](assets/db-wide-column.png)

Los elementos de datos se estructuran en columnas, en donde el tipo de dato puede variar. Algunos lo interpretan como un *Key-Value bidimensional*.

### Casos de uso

- Data science

### Implementaciones

- [Cassandra](https://cassandra.apache.org/)
- [BigTable](https://cloud.google.com/bigtable/)

## Graph

![graph](assets/db-graph.png)

La teoría de *grafos* aplicada al almacenamiento de datos. Estructura la información por medio de nodos(nodes) y bordes(edges)

### Casos de uso

- Análisis de datos
- Ciencia de datos

### Implementaciones

- [Neo4j](https://neo4j.com)

## Fuentes

* [https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf)
* [https://en.wikipedia.org/wiki/NoSQL](https://en.wikipedia.org/wiki/NoSQL)
* [https://en.wikipedia.org/wiki/Strozzi_NoSQL](https://en.wikipedia.org/wiki/Strozzi_NoSQL)
* [https://en.wikipedia.org/wiki/Key–value_database](https://en.wikipedia.org/wiki/Key–value_database)
* [https://en.wikipedia.org/wiki/Document-oriented_database](https://en.wikipedia.org/wiki/Document-oriented_database)
* [https://en.wikipedia.org/wiki/Wide-column_store](https://en.wikipedia.org/wiki/Wide-column_store)
* [http://www.leavcom.com/pdf/NoSQL.pdf](http://www.leavcom.com/pdf/NoSQL.pdf)
* [https://db-engines.com/en/ranking](https://db-engines.com/en/ranking)
* [https://www.mongodb.com/nosql-explained](https://www.mongodb.com/nosql-explained)
* [https://www.mongodb.com/scale/types-of-nosql-databases](https://www.mongodb.com/scale/types-of-nosql-databases)
* [https://www.mongodb.com/nosql-explained/nosql-vs-sql](https://www.mongodb.com/nosql-explained/nosql-vs-sql)
* [https://www.mongodb.com/compare/mongodb-postgresql](https://www.mongodb.com/compare/mongodb-postgresql)
