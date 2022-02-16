# Turbo

Nos ofrece la velocidad de  Single Page Application sin tener que escribir JavaScript. Turbo acelera los enlaces y los envíos de formularios sin necesidad de cambiar el HTML generado por el servidor.

Turbo está compuesto por:

## Turbo Drive

Nos permite navegar dentro de un proceso persistente y en segundo plano de forma asíncrona, lo que quiere decir es que podemos en un proceso llamar el contenido que queremos mostrar con un href sin tener que recargar la página completa.
Turbo drive requiere la nueva sección usando FETCH lo cual nos permite comenzar el proceso de traer el recurso que necesitamos. Este retorna una promesa la cual será completada cuando la respuesta esté disponible, lo mismo se usa para los FORMs. Este desarrollo se basa en lo que usábamos anteriormente para no recargar toda la pagina, usando una petición AJAX(Javascript asíncrono).


## Turbo frames

Con turbo frame podemos remplazar elementos independientes dentro de frames. Quiere decir que todo lo que pase dentro del frame(clicks, links o submitting forms) pasa dentro del frame, manteniendo el resto de la pagina sin recargar o cambiar.

Turbo busca y extrae el frame correspondiente de la respuesta resultante e intercambia el contenido en ese lugar. Esto puede sonar muy similar a un Iframe, pero en este caso todo el contenido de los frames son parte del mismo DOM, todo está bajo las misma aplicación sin comprometer o tener restricciones de seguridad. Además turbo frame nos ofrece:

- Cache
  puedes cachear los frames y expiraran cuando detecte un cambio
- Ejecución paralela
  Cada frame es generado por su propio HTTP request/response lo que significa que los procesos son manejados por separado.

Ejemplos de turbo frame tags en una applicación de Rails:

`app/views/profiles/show.html.erb`
```
<section>
  <h2>Emergency Info</h2>
  <%= turbo_frame_tag :emergency_info, src: new_emergency_info_path %>
</section>

```

`app/views/emergency_infos/new.html.erb`
```
<%= turbo_frame_tag :emergency_info do %>
  <%= render 'form', emergency_info: @emergency_info %>
<% end %>
```

Recordemos que al hacer submit del formulario abierto por la acción new llamará la acción create, y este al crear el emergency info nos redireccionara al show, si queremos mantener todo en el mismo frame también tenemos que agregar el turbo frame tag en la vista de show

`app/controllers/emergency_infos_controller.rb`
```
def create
  @emergency_info = EmergencyInfo.new(params)
  @emergency_info.profile = current_profile
  respond_to do |format|
    if @emergency_info.save
      format.html { redirect_to emergency_info_path, notice: "Emergency info was successfully created." }
    else
      format.html { render :new, status: :unprocessable_entity }
    end
  end
end
```

`app/views/emergency_infos/show.html.erb`
```
<%= turbo_frame_tag :emergency_info do %>
  <p><%= link_to 'Edit', edit_emergency_info_path %></p>
  <p>
    <strong>Allergies: </strong><%= @emergency_info.allergies %><br>
    <strong>Blood type: </strong><%= @emergency_info.blood_type %><br>
  </p>
<% end %>
```

Puedes ver el comportamiento ![aquí](http://g.recordit.co/uOlrvTo0EY.gif)

## Turbo Streams

Turbo streams nos permite cambiar cualquier parte de la página en respuesta a actualizaciones enviadas a través de una conexión Websocket o atraves de respuestas HTTP. Con respuestas HTTP, cada elemento del flujo especifíca una acción junto con un ID de destino para declarar lo que debería suceder con el HTML dentro de él.
Con turbo streams tenemos 7 acciones(append, prepend, replace update, remove, before, after)

Para ver turbo streams en acción, es necesario enviar el MIME type `turbo_stream` desde el controlador, el cual va a ejecutar la acción declarada. Para eso tenemos que declarar en views la acción que manda a llamar y con el formato turbo_stream, como en el siguiente archivo:  `app/views/family_members/create.turbo_stream.erb`. Vemos que se realiza una acción append, el cual busca un id llamado `family_members` al cual le va a hacer append del elemento retornado.


`app/views/profiles/show.html.erb`
```
<section>
  <h2>Family Members</h2>
  <div id="family_members">
  </div>
</section>
```

`app/controllers/family_members_controller.rb`
```
def create
  @family_member = FamilyMember.new(params)
  @family_member.profile = current_profile

  respond_to do |format|
    if @family_member.save
      format.turbo_stream
      format.html { redirect_to @family_member, notice: "Family member was successfully created." }
    else
      format.html { render :new, status: :unprocessable_entity }
    end
  end
end
```

`app/views/family_members/create.turbo_stream.erb`
```
<%= turbo_stream.append :family_members, @family_member %>
```

Puedes ver el comportamiento ![aquí](http://g.recordit.co/0yJJspUEVX.gif)

También turbo streams te permite usar una conexión websocket para reproducir el ejemplo anterior.

Para más información lee la documentación de la [página oficial](https://turbo.hotwired.dev)
