# CSV

Los archivos separados por comas (CSV, por sus siglas en Inglés) son comúnmente usados para intercambiar información entre programas de hojas de cálculo.

Representan la información de una tabla en un archivo de texto en donde los valores están separados por una coma, mientras que cada salto de línea corresponde a una fila de la tabla.

Una tabla sobre modelos de auto seria así en un archivo CSV

```
Year,Make,Model,Price
1997,Ford,E350,3000.00
1999,Chevy,Venture,4900.00
1996,Jeep,Grand Cherokee,4799.00
```

# Implementacion Ruby

## Clase CSV

La librería estandar cuenta con manejo de CSV por medio de una simple clase que ofrece métodos para generar y leer archivos CSV.

```
require 'csv'
```

Para obtener un arreglo a partir de un objeto String, se utiliza el método `parse`

```
string = "foo,0\nbar,1\nbaz,2\n"
CSV.parse(string) # => [["foo", "0"], ["bar", "1"], ["baz", "2"]]
```

mientras que para obtener un String a partir de un arreglo, se utiliza el método `generate`

```
output_string = CSV.generate do |csv|
 csv << ['foo', 0]
 csv << ['bar', 1]
 csv << ['baz', 2]
end
output_string # => "foo,0\nbar,1\nbaz,2\n"
```

[CSV](https://ruby-doc.org/stdlib-3.1.0/libdoc/csv/rdoc/CSV.html)

## Exportar un archivo CSV en Rails

Es posible colocar un enlace para descargar el archivo CSV utilizando `link_to` y especificando en formato de la ruta como csv.

```erb
 <%= link_to "Download CSV", products_path(format: "csv") %>
```

Utilizando `respond_to` en el método correspondiente del controlador es posible definir la acción a realizar al recibir una petición en formato csv, el metodo `send_data` permite mostrar la respuesta del navegador como un archivo adjunto el cual será descargado, este método recibe como argumento la cadena de texto que contiene la información a almacenar en formato csv.

```ruby
 ...
 respond_to do |format|
   format.html
   format.csv { send_data @products.to_csv }
 end
 ...
```

Y dentro del modelo se podria implementar un metodo

`models\product.rb`
```ruby
  def to_csv(options = {})
   CSV.generate(options) do |csv|
     csv << column_names
     all.each do |product|
       csv << product.attributes.values_at(*column_names)
     end
   end
  end
```

[Exporting CSV](http://railscasts.com/episodes/362-exporting-csv-and-excel)

# Consideraciones

La primer línea puede contener los nombres de cada una de las columnas, la presencia de esta línea se indica en el parámetro opcional `headers`
```
str = <<-EOT
Name,Value
foo,0
bar,1
baz,2
EOT

table = CSV.parse(str, headers: true)
table.headers # => ["Name", "Value"]
```

De la misma manera, se pueden agregar los nombres de las columnas.
```
data = CSV.parse('Bob,Engineering,1000', headers: %[name department salary])
data.first      #=> #<CSV::Row name:"Bob" department:"Engineering" salary:"1000">
```
