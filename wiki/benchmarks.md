# Benchmarks

Si deseamos saber si una función o implementación es mas rápida que otra, normalmente hacemos una prueba de referencia (Benchmark)

## Biblioteca estándar

Por default ruby provee del modulo de `benchmark` el cual nos permite hacer pruebas de referencia de una manera fácil y sencilla.

```ruby
require 'benchmark'

n = 5_000
Benchmark.bm(7) do |x|
  x.report("<<") do
    s = ""
    n.times do
      s << "a"
    end
  end
  x.report("+=") do
    s = ""
    n.times do
      s += "a"
    end
  end
end
```

Sin embargo, no existe una forma clara para determinar cuantas iteraciones por segundo son posibles, y a menudo solo ponemos un valor arbitrario para el numero de iteraciones a probar.

## IPS

La gema [benchmark-ips](https://rubygems.org/gems/benchmark-ips) resuelve este problema al correr la prueba durante un periodo de calentamiento, con el cual puede determinar cuantas operaciones por segundo son posibles

```ruby
require 'benchmark/ips'

Benchmark.ips do |x|
  x.report("<<") do
    s = ""
    s << "a"
  end
  x.report("+=") do
    s = ""
    s += "a"
  end

  x.compare!
end
```

## Memoria

Ahora si deseamos correr la prueba para determinar su uso de memoria, la gema [benchmark-memory](https://rubygems.org/gems/benchmark-memory) nos resuelve este problema

```ruby
require 'benchmark/memory'

Benchmark.memory do |x|
  x.report("<<") do
    s = ""
    s << "a"
  end
  x.report("+=") do
    s = ""
    s += "a"
  end

  x.compare!
end
```
