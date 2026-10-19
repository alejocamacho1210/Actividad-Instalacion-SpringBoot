# Actividad práctica: Mi primer proyecto con Spring Boot

**Estudiante:** Alejandro Camacho

Aplicación Spring Boot (Maven, Java 17+) con tres endpoints REST que aplica controladores, servicios, inyección de dependencias y configuración externa.

## Tecnologías
- Java 17+
- Spring Boot 4.0.0 (Spring Web MVC)
- Maven
- IDE: NetBeans
- Pruebas: Postman e Insomnia

## Ejecutar
```bash
mvn spring-boot:run
```
O desde el IDE: botón ▶ junto a `HolamundoApplication`. Por defecto la app queda en `http://localhost:8080`. Si el puerto está ocupado:
```bash
mvn spring-boot:run -Dspring-boot.run.arguments=--server.port=8082
```

## Endpoints
| Método | Endpoint | Status | Respuesta |
|---|---|---|---|
| GET | `/api/hola` | 200 OK | ¡Hola Mundo desde Spring Boot! |
| GET | `/saludo` | 200 OK | ¡Hola desde el servicio! |
| GET | `/config` | 200 OK | ¡Bienvenido a Spring Boot! |

## Estructura del proyecto
```
src/main/java/com/ejemplo/holamundo/
├── HolamundoApplication.java
├── controller/
│   ├── HolaMundoController.java   -> /api/hola
│   ├── SaludoController.java      -> /saludo (usa MiServicio por inyección)
│   └── ConfigController.java      -> /config (usa @Value)
└── service/MiServicio.java        -> @Service
src/main/resources/application.properties
capturas/                          -> evidencias de la actividad
```

## Conceptos aplicados
- `@RestController` / `@GetMapping`: exponen los endpoints.
- `@Service` + inyección por constructor: Spring crea `MiServicio` y lo entrega al controlador sin `new`.
- `@Value`: lee `mensaje.bienvenida` desde `application.properties`.

---

# Entregables

## 1. Estructura del proyecto en el IDE
![Estructura del proyecto](holamundo/holamundo/capturas/01-estructura-proyecto.png)

## 2. Consola con `Tomcat started on port ...`
![Consola con Tomcat iniciado](holamundo/holamundo/capturas/02-consola-tomcat-started.png)

## 3. Respuestas en Postman (Status 200 OK)

**`GET /api/hola`**

Pendiente: agregar captura como `holamundo/holamundo/capturas/05-postman-api-hola.png`.

**`GET /saludo`**

![Postman /saludo](holamundo/holamundo/capturas/03-postman-saludo.png)

**`GET /config`**

![Postman /config](holamundo/holamundo/capturas/04-postman-config.png)

## 4. Respuestas en Insomnia (Status 200 OK)

**`GET /api/hola`**

Pendiente: agregar captura como `holamundo/holamundo/capturas/06-insomnia-api-hola.png`.

**`GET /saludo`**

Pendiente: agregar captura como `holamundo/holamundo/capturas/07-insomnia-saludo.png`.

**`GET /config`**

Pendiente: agregar captura como `holamundo/holamundo/capturas/08-insomnia-config.png`.

## 5. Reto: ¿qué pasa con un método equivocado?
Al enviar un **POST** a `/api/hola` se obtiene **405 Method Not Allowed**, porque el controlador solo tiene `@GetMapping`. La URL y el método van juntos.

Pendiente: agregar la captura como `holamundo/holamundo/capturas/09-405-method-not-allowed.png`.

## 6. Reto: ¿para qué sirve `@Service`?
Al quitar temporalmente `@Service` en `MiServicio`, la aplicación no arranca y aparece un error como este:

```
Parameter 0 of constructor in SaludoController required a bean
of type 'MiServicio' that could not be found.
```

Significa que el controlador necesita un `MiServicio`, pero Spring no lo creó. Al volver a poner `@Service`, todo funciona.

Pendiente: agregar la captura como `holamundo/holamundo/capturas/10-error-sin-service.png`.

## 7. Respuesta breve
**¿Qué diferencia hay entre `@RestController` y `@Service`, y por qué Spring puede entregar el servicio al controlador sin usar `new`?**

`@RestController` marca una clase como controlador web: recibe peticiones HTTP (por ejemplo `GET /saludo`) y devuelve la respuesta directamente como cuerpo, en texto o JSON. `@Service` marca una clase con la lógica de negocio y la registra como *bean* dentro del contenedor de Spring.

Spring puede entregar el servicio al controlador sin `new` gracias a la inyección de dependencias. Al arrancar, escanea el paquete `com.ejemplo.holamundo`, crea un objeto de `MiServicio` porque tiene `@Service` y lo pasa por el constructor de `SaludoController`. El controlador solo declara qué necesita y Spring se encarga de crearlo y entregarlo. Por eso, si se quita `@Service`, la aplicación no arranca: no existe ningún bean de tipo `MiServicio` para inyectar.
