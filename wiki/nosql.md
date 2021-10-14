# NoSQL

Se entiende por NoSQL a aquellas bases de datos que no utilizan el modelo relacional como paradigma
principal para el manejo de datos.

El termino NoSQL fue acuñado a finales de la decada de los 2000's. Sin embargo desde los 60's se almacenan datos, y es hasta finales de los 70's cuando surge el concepto de SQL con el paper de Edgar Codd, "A Relational Model of Data for Large Shared Data Banks"

# Tipos de bases de datos NoSQL

Existen cuatro tipos de NoSQL:
* **Key-Value**: Cada elemento de datos en la base de datos se almacena como un par llave-valor. Algo muy similar a un *hashmap*, pero persistente.
* **Document**: Mantiene una estructura de datos similar a un formato tipo *JSON* en donde permite anidar documentos.
* **Wide Column**: Los elementos de datos se estructuran en columnas, en donde el tipo de dato puede variar. Algunos lo interpretan como un *Key-Value bidimensional*.
* **Graph**: La teoría de *grafos* aplicada al almacenamiento de datos. Estructura la información por medio de nodos(nodes) y bordes(edges)

## Tabla Comparativa
|-|---|-----|
| |SQL|NoSQL|
|-|---|-----|
|Modelo|tablas con filas y columnas fijas|Documento: documentos JSON, key-value: pares llave-valor, wide-column: tablas con filas y columnas dinámicas, graph: nodos y bordes|
|Historia|Desarrollado en los 1970's con un enfoque en reducir la duplicación de datos|Desarrollado a finales de la década de 2000 con un enfoque en escalar y permitir un cambio rápido de aplicaciones impulsado por prácticas ágiles y DevOps.|
|Implementaciones|Oracle, MySQL, Microsoft SQL Server, and PostgreSQL|Document: MongoDB and CouchDB, Key-value: Redis and DynamoDB, Wide-column: Cassandra and HBase, Graph: Neo4j and Amazon Neptune|
|Propósito primario|Propósito general|Document: propósito general, key-value: grandes cantidades de datos con consultas de búsqueda simples, Wide-column: grandes cantidades de datos con patrones de consulta predecibles, Graphs: análisis y análisis de relaciones entre datos conectados|
|Esquemas|Rígido|Flexible|
|Escalamiento|Vertical|Horizontal|



## Prinicipales Implementaciones
 * [Redis](https://redis.io) (key-value)
 * [MongoDB](https://www.mongodb.com) (document)
 * [Firebase](https://firebase.google.com) (document)
 * [Cassandra](https://cassandra.apache.org/) (wide-column)
 * [BigTable](https://cloud.google.com/bigtable/) (wide-column)
 * [Neo4j](https://neo4j.com) (graph)

## Casos de uso

* **Key-Value**: cache, busquedas simples, datos transaccionales y de acceso rápido
* **Document**: propósito general y estructuras de datos dinámicas
* **Wide Column**: big data
* **Graph**: análisis de datos y data science

## Alternativas
* [JSON(PostgresSQL)](https://www.postgresql.org/docs/current/datatype-json.html): Una alternativa a MongoDB es dentro PostgresSQL utilizando columnas de tipo JSON y JSONB. Las cuales habilitan la flexibilidad de datos que tienen las NoSQL basadas en documentos.
* [ETS(Erlang)](https://beta.erlang.org/docs/19/man/ets.html): Alternativa a key-value dentro del ecosistema de Erlang/Elixir
* [MNesia(Erlang)](http://erlang.org/doc/man/mnesia.html): Alternativa a key-value y SQL dentro del ecosistema de Erlang/Elixir

## Bibliografia

* https://en.wikipedia.org/wiki/NoSQL
* https://en.wikipedia.org/wiki/Strozzi_NoSQL
* https://en.wikipedia.org/wiki/Key–value_database
* https://en.wikipedia.org/wiki/Document-oriented_database
* https://en.wikipedia.org/wiki/Wide-column_store
* http://www.leavcom.com/pdf/NoSQL.pdf
* https://db-engines.com/en/ranking
* https://www.mongodb.com/nosql-explained
* https://www.mongodb.com/scale/types-of-nosql-databases
* https://www.mongodb.com/nosql-explained/nosql-vs-sql
* https://www.mongodb.com/compare/mongodb-postgresql
