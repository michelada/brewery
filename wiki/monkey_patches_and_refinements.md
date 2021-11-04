# Extendiendo el core de Ruby

Ruby es un lenguaje extremadamente dinamico al punto que nos permite extender clases del core(que estan escritas en C) con unas cuantas lineas de ruby.

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

## Refinements

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

Process.pal?("anita lava la tina")
=> true
"anita lava la tina".palindrome?

```
