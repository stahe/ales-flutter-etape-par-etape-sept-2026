# Introducción paso a paso al framework móvil Flutter

📖 **Lea el tutorial: [https://stahe.github.io/ales-flutter-etape-par-etape-sept-2026/](https://stahe.github.io/ales-flutter-etape-par-etape-sept-2026/)**

Este curso enseña a construir una aplicación **móvil** con el framework [Flutter](https://flutter.dev) 3.44 y el lenguaje [Dart](https://dart.dev) 3.12: una aplicación Android cuyas pantallas se generan **en el teléfono** a partir de datos JSON provenientes de un servidor. El mismo código también genera una aplicación web.

Sigue el esquema del curso [Introducción paso a paso al framework React](https://stahe.github.io/ales-react-etape-par-etape-sept-2026/) (así como el de los cursos de [Vue.js](https://stahe.github.io/ales-vuejs-etape-par-etape-sept-2026/) y [Angular](https://stahe.github.io/ales-angular-etape-par-etape-sept-2026/)): mismo servidor, mismas pantallas, escritas al estilo de Flutter, para un teléfono.

| Curso React | Curso Flutter |
|---|---|
| una aplicación web, en el navegador | una aplicación móvil (Android) y web |
| componentes funcionales, JSX, hooks | widgets, un método `build()`, `setState` |
| HTML + CSS (Bootstrap) | el motor de Flutter dibuja cada píxel (Material 3) |
| React Router | go_router |
| Zustand, TanStack Query | provider (`ChangeNotifier`), un pequeño caché |
| i18next | diccionarios JSON, `intl`, `flutter_localizations` |
| el navegador almacena la cookie del token | un cliente HTTP que gestiona las cookies (en el teléfono) |
| `localStorage` | `shared_preferences` |

El servidor, en cambio, sigue siendo el mismo: es el servidor JSON de la aplicación **RdvMedecins**, ya utilizado por los clientes React, Vue.js y Angular.

## El enfoque: muchos ejemplos breves y luego un caso de estudio

Flutter exige aprender muchos conceptos a la vez (un lenguaje, widgets, un ciclo de vida, programación asíncrona): por eso el curso se estructura en torno a **25 ejemplos breves**, cada uno centrado en un único concepto. Forman un único proyecto Flutter: basta con un solo comando `flutter pub get`, seguido de `flutter run -t lib/<ejemplo>/main.dart` para lanzar uno.

| Capítulo | Contenido | Ejemplos |
|---|---|---|
| Primeros pasos | un proyecto Flutter, `StatelessWidget` / `StatefulWidget`, `setState`, el árbol de widgets, eventos, todos los tipos de campos, un reducer (clases selladas, `switch`), validación, `Form` y `FormField`, formateo (`intl`) | 01–09 |
| Widgets | parámetros, funciones como parámetros (`ValueChanged`), composición (`child`, builders, widgets genéricos), ciclo de vida (`initState`, `didUpdateWidget`, `dispose`), `InheritedWidget`, mixins y animaciones, cuadro de diálogo de confirmación (`showDialog`) | 10–16 |
| Enrutamiento | go_router 16: rutas, parámetros, `ShellRoute` + `NavigationBar`, carga diferida, `redirect`, guardias | 17–18 |
| Programación asíncrona y estado compartido | `Future`, `Stream`, `async*`, temporizadores, antirrebote, respuestas obsoletas, `FutureBuilder`, `StreamBuilder`, provider, `shared_preferences`, tema oscuro | 19–20 |
| Internacionalización | diccionarios JSON, configuración, plurales (`Intl.pluralLogic`), fechas, importes, calendario traducido | 21 |
| El servidor: una caja negra | puesta en marcha del servidor JSON, su API, 48 ejemplos con `curl`, qué cambia para un cliente móvil | – |
| Comunicación con el servidor | el paquete `http`, la dirección del servidor (emulador, teléfono, web), la cookie del token en el teléfono, una capa de acceso a la API, un patrón modelo-vista (MVVM), errores del servidor asociados a los campos | 22–25 |

Cada ejemplo se presenta con su código completo, comentado línea por línea, y capturas de pantalla de su ejecución.

## El servidor: una caja negra

El servidor es el servidor NestJS de las lecciones anteriores, cuyos controladores devuelven **JSON**. Esta lección lo trata como una **caja negra**: se pone en marcha, se estudia su API y se consulta con `curl`, pero no es necesario leer su código (que se proporciona y está comentado para los curiosos).

- Todos los errores tienen el mismo formato: `{ "statusCode": 409, "cle": "ERRORS.LOGIN_TAKEN", "params": {...}, "champs": {...} }` — **claves** de traducción, nunca texto literal;
- autenticación mediante un token JWT en una cookie `httpOnly`: el navegador la almacena automáticamente; en un teléfono, la aplicación la almacena y la reenvía;
- un “modo de prueba” para el CAPTCHA que permite consultar la API con `curl` (y generar capturas de pantalla mediante pruebas automatizadas).

## El caso de estudio: el cliente Flutter de RdvMedecins

Una aplicación completa para **reservar citas en un consultorio médico**, con **todos** sus archivos (unos treinta) listados y comentados.

- **Flutter moderno**: Material 3, Dart 3 (records, filtrado, clases selladas, expresiones `switch`, extensiones), go_router, provider, una arquitectura en capas (datos, estado, interfaz).
- **Tres roles**: `ADMIN` (administra médicos y clientes), `DOCTOR` (reserva y cancela citas), `USER` (el paciente: reserva citas para sí mismo y administra su cuenta).
- **Confidencialidad**: un paciente nunca recibe los nombres de otros pacientes; el servidor no los envía.
- **Todo el estado de la pantalla en la URL**: `/agenda?idMedecin=1&jour=2026-10-05&reserver=7`; en un teléfono, el botón Atrás cierra la ventana de reserva; en la web, F5 y Atrás/Adelante funcionan.
- **Validación del lado del servidor**: los formularios muestran bajo cada campo los errores devueltos por la API; bloqueo optimista, nombres duplicados, login ya en uso, etc.
- **Sesión**: persiste entre sesiones (la cookie se almacena en el dispositivo), se restaura al iniciar (`GET /api/auth/moi`) y la expiración se gestiona en un único lugar (respuesta 401).
- **Optimizado para móviles**: menú lateral desplegable (o menú fijo en pantallas grandes), listas en lugar de tablas, administrador de contraseñas, zona de errores sobre la barra de navegación.
- **Francés / inglés**, incluido el calendario y los textos propios de Flutter.
- **Despliegue**: la aplicación Android (APK) y la versión web compilada, servida por el propio servidor JSON.

## Contenido del repositorio

```
exemples_flutter/          los 25 ejemplos breves (un único proyecto Flutter)
rdvmedecins-nestjs-json/   el servidor JSON de RdvMedecins (la “caja negra”)
rdvmedecins_flutter/       el cliente Flutter del caso de estudio
```

Las carpetas específicas de cada plataforma (`android/`, `web/`...) las genera `flutter create` (véase el curso).

## Tecnologías

Flutter 3.44 · Dart 3.12 · Material 3 · go_router 16 · provider 6 · http 1.5 · shared_preferences 2.5 · flutter_svg 2 · intl · flutter_localizations · flutter_test · del lado del servidor: NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Requisitos previos

- Conocimientos básicos de programación orientada a objetos (Java, C#, TypeScript, etc.) y del protocolo HTTP. El curso de [Dart](https://stahe.github.io/dart-sept-2026/) es una buena preparación, pero los conceptos de Dart utilizados se explican a lo largo de los ejemplos.
- El SDK de Flutter, Visual Studio Code y su extensión de Flutter, Android Studio (para el SDK de Android y el emulador) o un teléfono Android, Chrome; Node.js y un servidor MySQL (por ejemplo, Laragon en Windows) para el servidor JSON. Las instrucciones de instalación se proporcionan en los apéndices del curso.

## Autor

Este curso, sus ejemplos, el cliente Flutter y la adaptación del servidor JSON fueron escritos por **Claude**, la IA de [Anthropic](https://www.anthropic.com) (septiembre de 2026), a petición de **Serge Tahé**.
