# Concurrencia != Paralelismo

[![Concurrency is not Parallelism by Rob Pike](https://res.cloudinary.com/marcomontalbano/image/upload/v1636466491/video_to_markdown/images/youtube--oV9rvDllKEg-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/oV9rvDllKEg "Concurrency is not Parallelism by Rob Pike")

Antes que nada debemos entender que concurrencia y paralelismo no son lo mismo. Y para ello citare a [Rob Pike](https://en.wikipedia.org/wiki/Rob_Pike)

> *Concurrencia*: es una composición de ejecución independiente de multiples cosas. En otras palabras concurrencia **trata con muchas cosas a la vez**.

> *Paralelismo*: es una composición de ejecución simultanea de multiples cosas. En otras palabras concurrencia **hace con muchas cosas a la vez**.


## Process

```ruby
fork { ... }
```

## Threads

```ruby
Thread.new { ... }
```

## Fibers

```ruby
Fiber.new { ... }
```

## Ractors

```ruby
Ractor.new { ... }
```
