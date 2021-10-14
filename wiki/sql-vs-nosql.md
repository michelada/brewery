## Diferencias entre SQL y NoSQL

||SQL|NoSQL|
|Modelo|tablas con filas y columnas fijas|Documento: documentos JSON, key-value: pares llave-valor, wide-column: tablas con filas y columnas dinámicas, graph: nodos y bordes|
|Historia|Desarrollado en los 1970's con un enfoque en reducir la duplicación de datos|Desarrollado a finales de la década de 2000 con un enfoque en escalar y permitir un cambio rápido de aplicaciones impulsado por prácticas ágiles y DevOps.|
|Implementaciones|Oracle, MySQL, Microsoft SQL Server, and PostgreSQL|Document: MongoDB and CouchDB, Key-value: Redis and DynamoDB, Wide-column: Cassandra and HBase, Graph: Neo4j and Amazon Neptune|
|Propósito primario|Propósito general|Document: propósito general, key-value: grandes cantidades de datos con consultas de búsqueda simples, Wide-column: grandes cantidades de datos con patrones de consulta predecibles, Graphs: análisis y análisis de relaciones entre datos conectados|
|Esquemas|Rígido|Flexible|
|Escalamiento|Vertical|Horizontal|
|Transacciones ACID de múltiples registros|Soportado|La mayoría no admite transacciones ACID de varios registros. Sin embargo, algunos, como MongoDB, lo hacen.|
|Joins|Normalmente requerido|Normalmente no es necesario|
|Asignación de datos a objetos|Requiere ORM (mapeo relacional de objetos)|Muchos no requieren ORM. Los documentos de MongoDB se asignan directamente a las estructuras de datos en los lenguajes de programación más populares.|

[fuente](https://www.mongodb.com/nosql-explained/nosql-vs-sql#differences-between-sql-and-nosql)
