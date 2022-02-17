---
tags:
  - rails
  - middleware
---
# Flujo de procesamiento de Rails

Este es el flujo de procesamiento de un request en rails

- Una petición llega por el protocolo TCP al puerto (80 por default) de nuestro servidor web
- [rack](https://github.com/rack/rack) recibe la petición y reconoce que es un protocolo HTTP. Y por medio de una arquitectura de pipeline procesa la petición. A esto se le conoce como rack middleware, el cual consta de los siguientes:
  - ActionDispatch::HostAuthorization
  - Rack::Sendfile
  - ActionDispatch::Static
  - ActionDispatch::Executor
  - ActionDispatch::ServerTiming
  - ActiveSupport::Cache::Strategy::LocalCache::Middleware
  - Rack::Runtime
  - Rack::MethodOverride
  - ActionDispatch::RequestId
  - ActionDispatch::RemoteIp
  - Sprockets::Rails::QuietAssets
  - Rails::Rack::Logger
  - ActionDispatch::ShowExceptions
  - WebConsole::Middleware
  - ActionDispatch::DebugExceptions
  - ActionDispatch::ActionableExceptions
  - ActionDispatch::Reloader
  - ActionDispatch::Callbacks
  - ActiveRecord::Migration::CheckPending
  - ActionDispatch::Cookies
  - ActionDispatch::Session::CookieStore
  - ActionDispatch::Flash
  - ActionDispatch::ContentSecurityPolicy::Middleware
  - ActionDispatch::PermissionsPolicy::Middleware
  - Rack::Head
  - Rack::ConditionalGet
  - Rack::ETag
  - Rack::TempfileReaper
- Posteriormente el router recibe la petición, y según el *verbo* y el *path* de la url determina el controller y accion correspondientes.
- El controller ejecuta la peticion a la accion dada, y en caso de procesar exitosamente procede a la vista
- La vista construye la respuesta dado un template.
- El controller con el template generado de la vista retorna la respuesta de la petición.
