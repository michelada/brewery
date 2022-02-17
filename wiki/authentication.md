---
tags:
  - authentication
  - devise
---
# Autenticación

![authentication](assets/authentication-glyph.png)

> La validación de autenticidad de algo o alguien.

En desarrollo web se definiria como el mecanismo que cuenta nuestra aplicación para identificar a un usuario. Poniendolo en terminos mas simples, la autenticación funciona como puerta que delimita la interacción con el mundo exterior, y a la vez define la identidad de nuestros usuarios.

Por ende **un mecanismo de autenticación solo tiene la responsabilidad de identificar al usuario**. Esto se puede hacer mediante varios mecanismos, en desarrollo web el método principal es a través de cookies y para el caso de APIs web se utilizan tokens de autenticación.

## Implementación

Una aplicación de rails tenemos varias formas de implementar este mecanismo

- **HTTPAuthentication**: Cuando solo queremos restringir el acceso
- **Controllers + SecurePassword**: Cuando solo queremos una implementación simple para identificar al usuario
- **Warden + SecurePassword**: Similar a la anterior pero con acceso al usuario desde el middleware y con multiples estrategias.
- **Devise**: Cuando queremos una implementación completa y rápida de autenticación
- **JWT**: Cuando tenemos una API

### Basic HTTP Authentication

De manera nativa Rails, provee de [autenticación básica de http](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication). Solo es necesario incluir una linea de código en el controller que requiera authentication.

```ruby
  http_basic_authenticate_with name: "name", password: "secret", except: :index
```

Sin embargo esta solución no provee de una identidad del usuario, solo restringe el acceso.

### Controllers + ActiveModel::SecurePassword

Existen diferentes maneras de autenticar usuarios, la mas común es por medio de email/password.

A partir de Rails 3 que se integra `ActiveModel::SecurePassword` que utiliza [bcrypt-ruby](https://github.com/bcrypt-ruby/bcrypt-ruby) la cual un binding a el algoritmo de *password hashing* de OpenBSD.

```ruby
  # Gemfile
  gem "bcrypt"
```

#### Modelo

Dentro del modelo a autenticar agregaremos `has_secure_password` para incluir los métodos de autenticación.

```ruby
  # app/models/user.rb
  class User < ActiveRecord
    has_secure_password
  end
```

Por default este método utilizara la columna `password_digest` y `recovery_password_digest` en caso de implementar un mecanismo de recuperación de contraseña

#### Controllers

ApplicationController no cuenta con un mecanismo para autenticar, pero esto puede ser implementado fácilmente.
Para ello requerimos lo siguiente en `application_controller.rb`

```ruby
# app/controllers/application_controller.rb

class ApplicationController < ActionController::Base
  protected

  def auhtenticate!
    return if signed_in?

    head(:unauthorized) and redirect_to(new_session_path, alert: "Not Authorized")
  end

  def current_user
    @current_user ||= User.find(session[:user_id]) if session[:user_id]
  end
  helper_method :current_user

  def signed_in?
    current_user.present?
  end
  helper_method :signed_in?
end
```

Y un nuevo controller `sessions_controller.rb` cuya responsabilidad es manejar las sesiones de usuario.

```ruby
# app/controllers/sessions_controller.rb

class SessionsController < ApplicationController
  def new
  end

  def create
    user = User.find_by(email: params[:email])
    if user&.authenticate(params[:password])
      session[:user_id] = user.id
      redirect_to root_url, notice: "Welcome #{user.email}!"
    else
      render :new, alert: "Unable to find email or password"
    end
  end

  def destroy
    session[:user_id] = nil
    redirect_to root_url, notice: "Succesfully signed out"
  end
end
```

Como podemos ver informacion de session se almacena a nivel de controlador en `session[:user_id]`. Y esta queda encapsulada dentro de los métodos de `application_controller.rb`. Por lo que si requerimos autenticar un controller solo usaremos el siguiente `before_action`

```
  before_action :auhtenticate!
```

De esta manera aseguraremos que cada acción del controlador se identifique al usuario antes de ejecutarse.

### Middleware + ActiveModel::SecurePassword

En el escenario en donde requerimos la identidad del usuario antes de que el [pipeline de rails]({{ site.baseurl }}/rails-pipeline) llegue a los controllers, usaremos una implementación usando el middleware, esta se logra utilizando [warden](https://github.com/wardencommunity/warden). Warden es una gema que nos permite crear estrategias de autenticación a medida desde el rack middleware.

```ruby
# Gemfile
gem "warden"
```

### Configuracion

Continuando con el ejemplo anterior, solo moveríamos la lógica del controller a la estrategia de warden ubicado en un archivo de configuración.

```ruby
# config/initializers/warden.rb

Rails.application.config.middleware.use Warden::Manager do |manager|
  manager.default_strategies :password
  manager.failure_app = ->(env) do
    env['REQUEST_METHOD'] = 'GET'
    SessionsController.action(:new).call(env)
  end
end

Warden::Manager.serialize_into_session(&:id)

Warden::Manager.serialize_from_session do |id|
  User.find(id)
end

Warden::Strategies.add(:password) do
  def valid?
    params['email'] && params['password']
  end

  def authenticate!
    user = User.find_by(email: params['email'])
    if user&.authenticate(params['password'])
      success!(user)
    else
      fail("Invalid email or password")
    end
  end
end
```

#### Controllers

Con esta implementación los controladores se modifican para mover la responsabilidad de autenticación a warden.

```ruby
# app/controllers/application_controller.rb

class ApplicationController < ActionController::Base
  protected

  def authenticate!
    return if signed_in?

    head :unauthorized
  end

  def current_user
    warden.user
  end
  helper_method :current_user

  def signed_in?
    warden.authenticated?
  end
  helper_method :signed_in?

  def warden
    request.env['warden']
  end
end
```

```ruby
# app/controllers/sessions_controller.rb

class SessionsController < ApplicationController
  def new
  end

  def create
    warden.authenticate!
    redirect_to root_url, notice: "Welcome #{current_user.email}!"
  end

  def destroy
    warden.logout
    redirect_to root_url, notice: "Succesfully signed out"
  end
end
```

La ventaja de usar warden es que nos permite tener multiples estrategias de autenticación, eliminando así la necesidad de modificar codigo existente.

### Implementación completa

Sin embargo todo lo anterior es solo es una pequeña parte del flujo de autenticación, aun faltan flujos te recuperación de passwords, confirmación, invitaciones, tracking de sesiones, etc. Para ello la gema [devise](https://github.com/heartcombo/devise) nos ayuda a implementar todo esto sin la necesidad de escribir extensas lineas de código.

[Primeros pasos con devise](https://github.com/heartcombo/devise#getting-started)

La gran ventaja que tiene devise es que al estar implementada sobre warden, es que permite extender estrategias de autenticación. Ademas que cuenta con una gran variedad de módulos soportados por la comunidad, que habilitan funcionalidad adicional.

### APIS

En el caso de las APIs ya sean SOAP, REST o GraphQL. La verificación de los usuarios no utiliza cookies ya que las peticiones a estas suelen provenir de clientes fuera del domino de la aplicación.

#### Tokens

> Arreglo de caracteres pseudo aleatorios asociados a una entidad

Una de las formas mas simples de autenticar en una API es por medio de un token de usuario, el cual puede viajar en el header o la url de la petición http. Dando como resultado algo como lo siguiente

```
  curl https://api.example.com/posts?token=0082df2420494c5b8441c46bcdeec168
```

La desventaja de este método, es que pese a que el request viaje sobre https, las url y headers pueden ser espiados por medio de un sniffer y de esta forma comprometer la identidad de un usuario. Para ello es necesario proveer de mecanismos para la expiración de tokens.

#### JSON Web Tokens

Los JSON Web Tokens(JWT) son estándar abierto de la industria [RFC-7519](https://datatracker.ietf.org/doc/html/rfc7519) para la representación segura de entidades entre dos partes. La ventaja de JWT es que permite incluir información adicional en el payload del token, habilitando la capacidad de configurar un tiempo de expiración. Por lo que si un token JWT llegara a ser comprometido solo sería valido por un tiempo determinado.

[ver mas](https://jwt.io/)
