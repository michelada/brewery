# Extendiendo el core de Ruby

Ruby es un lenguaje extremadamente dinámico al punto que nos permite extender clases del core(que están escritas en C) con unas cuantas lineas de ruby.

## MonkeyPatch

La manera mas popular y menos segura son los famosos chango parches los cuales nos permiten extender clases de ruby como si escribiéramos nuestras propias clases.

```ruby
class String do
  def palindrome?
    str = self.dup.tr(" ", "")
    str == str.reverse
  end
end
```

Lo cual extiende la clase en todo el contexto de la ejecución.

```
irb(main)> "anita lava la tina".palindrome?
=> true
```

## Refinements

Por ello desde ruby 2.0 se implementaron **Refinements** una manera mas controlada de extender clases de ruby pero de manera encapsulada dentro de un componente.

```ruby
module Palindromizer
  refine NilClass do
    def palindrome? = false
  end

  refine String do
    def palindrome?
      str = self.dup.tr(" ", "")
      str == str.reverse
    end
  end
end

class Process
  using Palindromizer

  def self.pal?(str) = str.palindrome?
end
```

Al hacer esto nuestras extensión solo afecta a nuestra clase y mantiene el comportamiento original de classes core.

```
irb(main) > Process.pal?("anita lava la tina")
=> true
irb(main)> "anita lava la tina".palindrome?
=> undefined method `palindrome?' for "anita lava la tina":String (NoMethodError)
```
