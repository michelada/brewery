# SOLID

En ingeniería de software, SOLID es un acrónimo mnemónico de cinco principios de diseño destinados a hacer que los diseños de software sean más comprensibles, flexibles y fáciles de mantener. Los principios son un subconjunto de muchos principios promovidos por Robert C. Martin (unclebob), introducidos por primera vez en su artículo de 2000 Design Principles and Design Patterns.

## Single Responsibility

Establece que cada módulo debe tener una responsabilidad única, lo cual significa que tiene que ser altamente cohesivo e implementar una lógica fuertemente relacionada.

### Ejemplo

#### Malo
```ruby
class PlaceOrder
  def initialize(product: Product.new)
    @product = product
  end

  def call
    # 1. Lógica relacionada a verification
       # de disponibilidad en inventario
    # 2. Lógica relacionada a procesamiento de pago
    # 3. Lógica relacionada a procesamiento de envio
  end
end
 ```

#### Bueno

```ruby
class PlaceOrder
  def initialize(product: Product.new)
    @product = product
  end

  def call
    StockAvailability.new(@product).call
    ProductPayment.new(@product).call
    ProductShipment.new(@product).call
  end
end
```

## Open Closed Principle

Establece que los módulos de software deben estar abiertos para extensión, pero cerrados para modificación; es decir, que un modulo de este tipo puede permitir que su comportamiento se amplíe sin modificar su implementación.

### Ejemplo

#### Malo

```ruby
class EventTracker
  def initialize(logging_from)
    @logging_from = logging_from
  end

  def log(message)
    case @logging_from
    when :console
      print(message)
    when :file
      Disk.write("logs.txt", message: message)
    end
  end
end
```
#### Bueno

```ruby
class EventTracker
  def initialize(logger)
    @logger = logger
  end

  def log(message)
    @logger.log(message)
  end
end

class ConsoleLogger
  def log(message)
    print(message)
  end
end

class FileLogger
  def log(message)
    Disk.write("logs.txt", message: message)
  end
end
```

Nota: En los lenguajes de tipado estático es comun encontrar con interfaces o protocolos, las cuales son classes abstractas que definen los metodos que se invocan y por medio de herencia se determina si una instancia de objeto puede o no llamar a cierto metodo. En el caso de lenguajes de tipado dinamico como ruby o python esto no aplica ya que se aplica el [tipado de pato](https://en.wikipedia.org/wiki/Duck_typing).

## Liskov Subtitution Principle

Se define como una extensión del 'Open Closed Principle' que establece que las nuevas clases derivadas que extienden la clase base no deberían cambiar el comportamiento de la clase base (comportamiento de los métodos heredados). Siempre que una clase Y sea una subclase de la clase X, cualquier instancia que haga referencia a la clase X también debería poder hacer referencia a la clase Y (los tipos derivados deben ser sustituibles por sus tipos base).

### Ejemplo

#### Malo

```ruby
class Rectangle
  def initialize(width, height)
    @width = width
    @height = height
  end

  def width=(value)
    @width = value
  end

  def height=(value)
    @height = value
  end
end

class Square < Rectangle
  # Violación LSP: clase heredada sobrescribe comportamiento del padre

  def width=(value)
    super
    @height = value
  end

  def height=(value)
    super
    @width = value
  end
end
```

## Interface Segregation Principle

Establece que ningún cliente debe ser obligado a depender de los métodos que no usa. El ISP divide las interfaces que son muy grandes en otras más pequeñas y más específicas para que los clientes solo tengan que conocer los métodos que les interesan. Estas interfaces reducidas también se denominan interfaces de rol. Este principio está destinado a mantener un sistema desacoplado y, por lo tanto, más fácil de refactorizar, cambiar y redistribuir.

### Ejemplo

#### Malo

```ruby
class Car
  def open; end
  def start_engine; end
  def change_engine; end
end

# ISP violation: La instancia Driver
# no hace uso de #change_engine
class Drive
  def take_a_ride(car)
    car.open
    car.start_engine
  end
end

# ISP violation: La instancia Mechanic
# no hace uso de #start_engine
class Mechanic
  def repair(car)
    car.open
    car.change_engine
  end
end
```

#### Bueno

```ruby
module Openable
  def open; end
end

module Driveable
  def start_engine; end
end

module Fixable
  def change_engine; end
end


class Drive
  def take_a_ride(vehicle)
    raise "Vehicle is not Driveable" unless vehicle.is_a?(Driveable)

    vehicle.start_engine
  end
end

class Mechanic
  def repair(vehicle)
    raise "Vehicle is not Fixable" unless vehicle.is_a?(Fixable)

    vehicle.change_engine
  end
end
```


## Dependency Inversion Principle

Se refiere a una forma específica de desacoplar los módulos de software. Al seguir este principio, las relaciones de dependencia convencionales establecidas desde módulos de alto nivel de configuración de políticas a módulos de dependencia de bajo nivel se invierten, lo que hace que los módulos de alto nivel sean independientes de los detalles de implementación del módulo de bajo nivel. El principio establece:

  - *Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de las abstracciones.*
  - Las abstracciones no deben depender de los detalles. Los detalles deben depender de las abstracciones.


### Ejemplo

#### Malo

```ruby
class EventTracker
  def initialize
    # Una instancia de bajo nivel ConsoleLogger
    # es directamente creada dentro de una de alto nivel.
    # La clase EventTracker aumenta el acoplamiento
    @logger = ConsoleLogger.new
  end

  def log(message)
    @logger.log(message)
  end
end
```

#### Bueno

```ruby
class EventTracker
  def initialize(logger: ConsoleLogger.new)
    @logger = logger
  end

  # Por medio de injección de dependencias en el
  # Open Closed Principle
  def log(message)
    @logger.log(message)
  end
end
```
