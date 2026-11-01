# Quarkus a Fondo — Roadmap

Este documento define el mapa general del curso **Quarkus a Fondo**.

No es un índice inmutable. Es una guía de progresión pedagógica que debe revisarse conforme avance el curso, cambie Quarkus o la experimentación revele un orden mejor.

La intención no es recorrer todas las extensiones disponibles, sino construir conocimiento de manera progresiva:

> documentación oficial → comprensión → experimentación → demo aislada → aplicación real → video → código público

## Principios del roadmap

- La documentación oficial actual de Quarkus es la fuente principal.
- Cada sesión debe tener una **demo aislada** centrada únicamente en el tema.
- El mismo concepto debe incorporarse después al **Issue Tracker**, la aplicación transversal.
- El Issue Tracker no debe usar tecnologías o conceptos que todavía no hayan sido explicados.
- **Testing se introduce temprano y se mantiene durante todo el curso.**
- **La separación de responsabilidades se introduce progresivamente**, cuando exista funcionalidad suficiente para justificarla. No se crearán capas, paquetes o abstracciones vacías antes de que el curso haya explicado el problema que resuelven.
- **CDI se aprende progresivamente**: primero beans, scopes e inyección; después qualifiers, producers, interceptores, eventos y demás capacidades cuando aparezca una necesidad real.
- **Dev Services** se utiliza siempre que ayude a reducir infraestructura accidental durante desarrollo y pruebas.
- **Podman** será el runtime de contenedores recomendado para el entorno local, sin acoplar el código a Podman.
- **Messaging forma parte del tronco principal** del curso debido a su importancia en arquitecturas modernas y microservicios.
- Los ejemplos deben distinguir entre simplificaciones didácticas y decisiones que mantendríamos en una aplicación real.

## Aplicación transversal: Issue Tracker

El Issue Tracker evolucionará progresivamente para incorporar, cuando corresponda:

- proyectos;
- issues;
- estados;
- prioridades;
- comentarios;
- etiquetas;
- usuarios;
- asignaciones;
- miembros de proyectos;
- roles y permisos;
- historial;
- búsquedas y filtros;
- notificaciones;
- auditoría;
- integraciones externas;
- eventos de dominio.

La estructura del Issue Tracker debe crecer junto con las necesidades reales de la aplicación.

No se crearán desde el inicio paquetes o capas como `api`, `service`, `repository` o `domain` únicamente para anticipar una arquitectura futura. Cada separación debe aparecer cuando exista código y una responsabilidad concreta que justifiquen introducirla.

Cuando el curso llegue a CDI y comience a existir funcionalidad de negocio suficiente, se podrá introducir progresivamente una estructura como:

```text
api/
  dto/
  resource/

service/

repository/

domain/
```

La estructura `Resource → Service → Repository` no debe entenderse como una plantilla obligatoria para todas las sesiones. Su propósito es separar responsabilidades cuando estas realmente existan.

Antes de estudiar persistencia, una implementación como `InMemoryIssueRepository` podrá utilizarse una vez que el concepto de repositorio ya tenga sentido dentro de la aplicación. Cuando posteriormente se incorpore Hibernate ORM/Panache, la infraestructura podrá cambiar sin trasladar persistencia o reglas de negocio a los recursos REST.

---

# Bloque I — Fundamentos de Quarkus

## 01. Qué es Quarkus y qué problema intenta resolver

**Demo aislada:** `demos/01-getting-started/`

**Issue Tracker:** creación inicial del proyecto.

**Video:** [Ver la sesión 01 en YouTube](https://youtu.be/4he5y7Fr2TU?si=ou4iMOlWNRKypFGy)

**Objetivos principales:**
- posicionar Quarkus dentro del ecosistema Java;
- entender build time y runtime a nivel introductorio;
- conocer la filosofía de extensiones;
- ejecutar la primera aplicación.

**Fuentes oficiales:**
- https://quarkus.io/get-started/
- https://quarkus.io/guides/getting-started

## 02. Crear un proyecto: Maven, extensiones y estructura

**Demo aislada:** `demos/02-project-structure/`.

**Issue Tracker:** revisar el proyecto base creado en la sesión anterior y conocer su estructura.

**Fuente oficial:**
- https://quarkus.io/guides/maven-tooling

## 03. Dev Mode y ciclo de desarrollo

**Demo aislada:** modificar comportamiento mientras la aplicación está ejecutándose.

**Issue Tracker:** primer endpoint temporal y uso habitual de `quarkus:dev`.

**Fuentes oficiales:**
- https://quarkus.io/guides/maven-tooling
- https://quarkus.io/guides/dev-mode-differences

## 04. Dev UI

**Demo aislada:** explorar Dev UI y las extensiones instaladas.

**Issue Tracker:** utilizar Dev UI como herramienta recurrente de inspección.

**Fuente oficial:**
- https://quarkus.io/guides/dev-ui

## 05. Configuración con SmallRye Config

**Demo aislada:** `configuration-demo`.

**Issue Tracker:** configurar propiedades propias de la aplicación.

**Fuentes oficiales:**
- https://quarkus.io/guides/config-reference
- https://quarkus.io/guides/config

## 06. Perfiles y fuentes de configuración

**Demo aislada:** perfiles `dev`, `test`, `prod` y perfiles personalizados.

**Issue Tracker:** separar configuración por entorno sin duplicar código.

**Fuente oficial:**
- https://quarkus.io/guides/config-reference

## 07. CDI esencial: beans, scopes e inyección

CDI se introduce aquí únicamente hasta el nivel necesario para comenzar a diseñar la aplicación correctamente. Los temas avanzados aparecerán después cuando exista un problema concreto que resolver.

**Demo aislada:** `cdi-demo`.

**Issue Tracker:** comenzar separación Resource → Service → Repository e inyectar dependencias.

**Fuentes oficiales:**
- https://quarkus.io/guides/cdi
- https://quarkus.io/guides/cdi-reference

---

# Bloque II — APIs REST y diseño por capas

## 08. HTTP, Jakarta REST y Quarkus REST

**Demo aislada:** `rest-demo`.

**Issue Tracker:** primeros recursos `/projects` y `/issues`, todavía en memoria.

**Fuente oficial:**
- https://quarkus.io/guides/rest

## 09. JSON y Jackson

**Demo aislada:** `json-demo`.

**Issue Tracker:** representación JSON de Projects e Issues.

**Fuentes oficiales:**
- https://quarkus.io/guides/rest-json
- https://quarkus.io/guides/rest

## 10. DTO, records y límites entre capas

**Demo aislada:** `dto-demo`.

**Issue Tracker:** separar contratos HTTP del modelo interno.

**Fuente oficial principal:**
- https://quarkus.io/guides/rest

## 11. Parámetros, headers, status codes y Response

**Demo aislada:** ampliar `rest-demo`.

**Issue Tracker:** CRUD HTTP coherente para Projects e Issues.

**Fuente oficial:**
- https://quarkus.io/guides/rest

## 12. Validación con Jakarta Validation

**Demo aislada:** `validation-demo`.

**Issue Tracker:** validar creación y modificación de proyectos/issues.

**Fuente oficial:**
- https://quarkus.io/guides/validation

## 13. Manejo de errores y ExceptionMapper

**Demo aislada:** `error-handling-demo`.

**Issue Tracker:** contrato uniforme de errores.

**Fuente oficial:**
- https://quarkus.io/guides/rest

## 14. OpenAPI y Swagger UI

**Demo aislada:** `openapi-demo`.

**Issue Tracker:** documentar la API existente.

**Fuente oficial:**
- https://quarkus.io/guides/openapi-swaggerui

## 15. Filtros HTTP y CORS

**Demo aislada:** `rest-filters-demo`.

**Issue Tracker:** introducir únicamente las capacidades transversales necesarias.

**Fuentes oficiales:**
- https://quarkus.io/guides/rest
- https://quarkus.io/guides/security-cors

---

# Bloque III — Testing como práctica permanente

Testing se introduce aquí formalmente, pero a partir de este bloque acompaña a todas las capacidades nuevas.

## 16. JUnit y @QuarkusTest

**Demo aislada:** `testing-demo`.

**Issue Tracker:** tests de servicios y recursos existentes.

**Fuente oficial:**
- https://quarkus.io/guides/getting-started-testing

## 17. Testing HTTP con REST Assured

**Demo aislada:** ampliar `testing-demo`.

**Issue Tracker:** pruebas de API.

**Fuente oficial:**
- https://quarkus.io/guides/getting-started-testing

## 18. Mocking e @InjectMock

**Demo aislada:** `mocking-demo`.

**Issue Tracker:** aislar dependencias cuando corresponda.

**Fuentes oficiales:**
- https://quarkus.io/guides/getting-started-testing
- https://quarkus.io/guides/getting-started-testing#mock-support

## 19. Test Profiles

**Demo aislada:** `test-profile-demo`.

**Issue Tracker:** ejecutar escenarios con configuraciones alternativas.

**Fuente oficial:**
- https://quarkus.io/guides/getting-started-testing

---

# Bloque IV — Persistencia

## 20. Datasources, PostgreSQL y Dev Services

**Demo aislada:** `datasource-demo`.

**Issue Tracker:** primera base de datos real.

Dev Services será presentado como una capacidad recurrente del curso. Para desarrollo local se utilizará preferentemente Podman como runtime de contenedores.

**Fuentes oficiales:**
- https://quarkus.io/guides/datasource
- https://quarkus.io/guides/dev-services
- https://quarkus.io/guides/podman

## 21. Jakarta Persistence y Hibernate ORM

**Demo aislada:** `hibernate-orm-demo`.

**Issue Tracker:** persistir Projects e Issues.

**Fuente oficial:**
- https://quarkus.io/guides/hibernate-orm

## 22. Relaciones y modelado ORM

**Demo aislada:** ampliar `hibernate-orm-demo`.

**Issue Tracker:** modelar relaciones necesarias sin introducir todavía todo el dominio futuro.

**Fuente oficial:**
- https://quarkus.io/guides/hibernate-orm

## 23. Transacciones

**Demo aislada:** `transactions-demo`.

**Issue Tracker:** definir fronteras transaccionales en la capa de servicios.

**Fuente oficial:**
- https://quarkus.io/guides/transaction

## 24. Panache: qué simplifica y qué no

**Demo aislada:** `panache-demo`.

**Issue Tracker:** introducir Panache sobre el conocimiento previo de Hibernate ORM.

**Fuente oficial:**
- https://quarkus.io/guides/hibernate-orm-panache

## 25. Repository vs Active Record

**Demo aislada:** comparar ambas variantes en `panache-demo`.

**Issue Tracker:** mantener arquitectura por capas y tomar una decisión explícita.

**Fuente oficial:**
- https://quarkus.io/guides/hibernate-orm-panache

## 26. Consultas, paginación, sorting y filtros

**Demo aislada:** `panache-query-demo`.

**Issue Tracker:** búsquedas y listados de issues.

**Fuente oficial:**
- https://quarkus.io/guides/hibernate-orm-panache

## 27. Migraciones con Flyway

**Demo aislada:** `flyway-demo`.

**Issue Tracker:** esquema versionado.

**Fuente oficial:**
- https://quarkus.io/guides/flyway

## 28. Testing de persistencia y recursos externos

**Demo aislada:** `persistence-testing-demo`.

**Issue Tracker:** pruebas con PostgreSQL real proporcionado por Dev Services/Test Resources.

**Fuentes oficiales:**
- https://quarkus.io/guides/getting-started-testing
- https://quarkus.io/guides/dev-services

---

# Bloque V — Integraciones síncronas

## 29. Quarkus REST Client

**Demo aislada:** `rest-client-demo`.

**Issue Tracker:** primera integración con un servicio externo.

**Fuente oficial:**
- https://quarkus.io/guides/rest-client

## 30. Configuración avanzada del REST Client

**Demo aislada:** ampliar `rest-client-demo`.

**Issue Tracker:** timeouts, headers, providers y configuración del cliente.

**Fuente oficial:**
- https://quarkus.io/guides/rest-client

## 31. Manejo de errores remotos

**Demo aislada:** `rest-client-errors-demo`.

**Issue Tracker:** mapear fallos de servicios externos de manera controlada.

**Fuente oficial:**
- https://quarkus.io/guides/rest-client

## 32. Fault Tolerance

**Demo aislada:** `fault-tolerance-demo`.

**Issue Tracker:** aplicar timeout, retry, circuit breaker o fallback sólo donde exista una necesidad demostrable.

**Fuente oficial:**
- https://quarkus.io/guides/smallrye-fault-tolerance

---

# Bloque VI — Seguridad

La seguridad aparece cuando la aplicación ya necesita representar usuarios reales. Antes de este bloque no se introduce una autenticación provisional.

## 33. Arquitectura de seguridad en Quarkus

**Demo aislada:** `security-demo`.

**Issue Tracker:** preparar el modelo funcional para usuarios y membresías.

**Fuente oficial:**
- https://quarkus.io/guides/security-overview

## 34. OIDC, OAuth 2.0, OpenID Connect y JWT

**Demo aislada:** `oidc-demo`.

**Issue Tracker:** autenticación.

**Fuentes oficiales:**
- https://quarkus.io/guides/security-oidc-bearer-token-authentication
- https://quarkus.io/guides/security-oidc-code-flow-authentication

## 35. Keycloak y Dev Services

**Demo aislada:** ampliar `oidc-demo`.

**Issue Tracker:** IdP local reproducible mediante Dev Services.

**Fuente oficial:**
- https://quarkus.io/guides/security-openid-connect-dev-services

## 36. Roles, RBAC y SecurityIdentity

**Demo aislada:** `authorization-demo`.

**Issue Tracker:** roles como admin, member o maintainer.

**Fuente oficial:**
- https://quarkus.io/guides/security-authorize-web-endpoints-reference

## 37. Permisos y @PermissionsAllowed

**Demo aislada:** ampliar `authorization-demo`.

**Issue Tracker:** permisos explícitos más allá de roles globales.

**Fuente oficial:**
- https://quarkus.io/guides/security-authorize-web-endpoints-reference

## 38. Autorización contextual sobre recursos

**Demo aislada:** `resource-security-demo`.

**Issue Tracker:** un usuario sólo puede operar sobre proyectos donde tenga los permisos requeridos.

**Fuentes oficiales:**
- https://quarkus.io/guides/security-authorize-web-endpoints-reference
- documentación oficial de Keycloak cuando corresponda.

## 39. Testing de seguridad

**Demo aislada:** `security-testing-demo`.

**Issue Tracker:** tests de identidades, roles y permisos.

**Fuente oficial:**
- https://quarkus.io/guides/security-testing

---

# Bloque VII — Integración asíncrona, Messaging y eventos

Messaging forma parte del tronco principal. Primero se enseña el problema arquitectónico y después la tecnología.

## 40. De llamadas síncronas a eventos

**Demo aislada:** `events-concepts-demo`.

**Issue Tracker:** identificar eventos relevantes, por ejemplo `IssueCreated`.

**Objetivos:**
- evento vs comando;
- productor y consumidor;
- broker;
- desacoplamiento;
- cuándo no usar messaging.

**Fuentes oficiales principales:**
- https://quarkus.io/guides/messaging-overview
- https://quarkus.io/guides/messaging

## 41. Quarkus Messaging

**Demo aislada:** `messaging-demo`.

**Issue Tracker:** publicar y consumir el primer evento.

**Fuente oficial:**
- https://quarkus.io/guides/messaging

## 42. Apache Kafka con Quarkus

**Demo aislada:** `kafka-demo`.

**Issue Tracker:** broker real para eventos.

En local se utilizará preferentemente Dev Services + Podman.

**Fuentes oficiales:**
- https://quarkus.io/guides/kafka
- https://quarkus.io/guides/kafka-dev-services

## 43. Messaging confiable

**Demo aislada:** `reliable-messaging-demo`.

**Issue Tracker:** diseñar consumidores y productores teniendo en cuenta:
- acknowledgements;
- fallos;
- reintentos;
- dead-letter queues;
- duplicados;
- idempotencia;
- orden;
- offsets;
- semánticas de entrega.

**Fuentes oficiales:**
- https://quarkus.io/guides/messaging
- https://quarkus.io/guides/kafka

## 44. Patrones de integración con eventos

**Demo aislada:** `event-patterns-demo`.

**Issue Tracker:** evaluar e implementar un patrón apropiado, con especial interés en Transactional Outbox e Idempotent Consumer.

La explicación conceptual podrá complementarse con documentación oficial de los proyectos implicados cuando corresponda.

## 45. Del broker local a servicios administrados

**Demo aislada:** reutilizar `kafka-demo`.

**Issue Tracker:** mantener el código desacoplado de la infraestructura concreta.

El entorno local utilizará Apache Kafka. La arquitectura deberá dejar abierta la posibilidad de usar servicios Kafka compatibles o administrados, por ejemplo:

- Confluent Cloud;
- Azure Event Hubs mediante su compatibilidad con el protocolo Kafka.

Estas plataformas no serán requisito para seguir el curso.

---

# Bloque VIII — Modelos de ejecución y rendimiento

## 46. Blocking vs non-blocking

**Demo aislada:** `execution-model-demo`.

**Issue Tracker:** analizar endpoints ya existentes y entender dónde se ejecutan.

**Fuentes oficiales:**
- https://quarkus.io/guides/rest
- https://quarkus.io/guides/quarkus-reactive-architecture

## 47. Mutiny: Uni y Multi

**Demo aislada:** `mutiny-demo`.

**Issue Tracker:** aplicar Mutiny únicamente a un caso donde aporte valor.

**Fuentes oficiales:**
- https://quarkus.io/guides/mutiny-primer
- https://smallrye.io/smallrye-mutiny/

## 48. Virtual Threads

**Demo aislada:** `virtual-threads-demo`.

**Issue Tracker:** comparar alternativas para workloads apropiados.

**Fuente oficial:**
- https://quarkus.io/guides/virtual-threads

## 49. Cache

**Demo aislada:** `cache-demo`.

**Issue Tracker:** cachear únicamente operaciones donde exista beneficio claro.

**Fuente oficial:**
- https://quarkus.io/guides/cache

## 50. Scheduler

**Demo aislada:** `scheduler-demo`.

**Issue Tracker:** tarea programada real, por ejemplo mantenimiento o notificaciones pendientes.

**Fuente oficial:**
- https://quarkus.io/guides/scheduler-reference

---

# Bloque IX — CDI avanzado cuando ya existe una razón para aprenderlo

CDI avanzado no se concentra al principio del curso. Algunos conceptos pueden haber aparecido antes de este bloque si una sesión concreta los necesitó. Este bloque sirve para consolidarlos y entender el modelo con mayor profundidad.

## 51. Qualifiers y múltiples implementaciones

**Demo aislada:** `cdi-qualifiers-demo`.

**Issue Tracker:** resolver una necesidad real de selección entre implementaciones.

**Fuente oficial:**
- https://quarkus.io/guides/cdi-reference

## 52. Producers y disposers

**Demo aislada:** `cdi-producers-demo`.

**Issue Tracker:** producir dependencias que no puedan modelarse simplemente como beans propios.

**Fuente oficial:**
- https://quarkus.io/guides/cdi-reference

## 53. Interceptors y cross-cutting concerns

**Demo aislada:** `cdi-interceptors-demo`.

**Issue Tracker:** aplicar un concern transversal apropiado sin ensuciar la lógica de negocio.

**Fuente oficial:**
- https://quarkus.io/guides/cdi-reference

## 54. Eventos CDI

**Demo aislada:** `cdi-events-demo`.

**Issue Tracker:** comparar eventos internos CDI con eventos distribuidos enviados mediante Messaging.

**Fuente oficial:**
- https://quarkus.io/guides/cdi-reference

---

# Bloque X — Observabilidad y operación

## 55. Logging

**Demo aislada:** `logging-demo`.

**Issue Tracker:** logging útil y consistente.

**Fuente oficial:**
- https://quarkus.io/guides/logging

## 56. Health

**Demo aislada:** `health-demo`.

**Issue Tracker:** liveness y readiness.

**Fuente oficial:**
- https://quarkus.io/guides/smallrye-health

## 57. Metrics con Micrometer

**Demo aislada:** `metrics-demo`.

**Issue Tracker:** métricas técnicas y, cuando tenga sentido, funcionales.

**Fuente oficial:**
- https://quarkus.io/guides/telemetry-micrometer

## 58. OpenTelemetry

**Demo aislada:** `opentelemetry-demo`.

**Issue Tracker:** trazabilidad de REST, persistencia e integraciones.

**Fuente oficial:**
- https://quarkus.io/guides/opentelemetry

## 59. Observabilidad integrada

**Demo aislada:** `observability-demo`.

**Issue Tracker:** correlacionar logs, métricas y traces.

**Fuente oficial:**
- https://quarkus.io/guides/observability

---

# Bloque XI — Empaquetado y producción

## 60. Empaquetado de aplicaciones Quarkus

**Demo aislada:** `packaging-demo`.

**Issue Tracker:** generar y entender el artefacto ejecutable.

**Fuente oficial:**
- https://quarkus.io/guides/maven-tooling

## 61. Container Images

**Demo aislada:** `container-demo`.

**Issue Tracker:** construir imagen de contenedor.

**Fuente oficial:**
- https://quarkus.io/guides/container-image

## 62. Configuración y secretos en producción

**Demo aislada:** `external-config-demo`.

**Issue Tracker:** configuración externa al artefacto.

**Fuentes oficiales:**
- https://quarkus.io/guides/config-reference
- https://quarkus.io/guides/kubernetes-config

## 63. Kubernetes

**Demo aislada:** `kubernetes-demo`.

**Issue Tracker:** generar y revisar recursos de despliegue.

**Fuente oficial:**
- https://quarkus.io/guides/deploying-to-kubernetes

## 64. Native executable

**Demo aislada:** `native-demo`.

**Issue Tracker:** construir y analizar una versión native cuando tenga sentido.

**Fuente oficial:**
- https://quarkus.io/guides/building-native-image

## 65. Testing de integración del artefacto final

**Demo aislada:** reutilizar las aplicaciones anteriores.

**Issue Tracker:** tests JVM, empaquetados, contenedor/native cuando corresponda.

**Fuente oficial:**
- https://quarkus.io/guides/getting-started-testing

## 66. Issue Tracker completo: del código a producción

Sesión de integración y cierre del recorrido troncal.

Debe mostrar cómo las capacidades aprendidas se relacionan y qué decisiones arquitectónicas fueron tomadas durante la evolución del proyecto.

---

# Ruta avanzada — Quarkus por dentro

Esta ruta no es requisito para completar el Issue Tracker. Su objetivo es entender qué hace diferente a Quarkus internamente.

Temas candidatos:

1. build time vs runtime;
2. bootstrap y augmentation;
3. ArC internals;
4. class loading;
5. anatomía de una extensión;
6. Build Items y Build Steps;
7. configuración de extensiones;
8. Dev UI para extensiones;
9. native image: reflection, resources y reachability;
10. creación de una extensión propia.

**Fuentes oficiales principales:**
- https://quarkus.io/guides/writing-extensions
- https://quarkus.io/guides/building-my-first-extension
- https://quarkus.io/guides/cdi-integration
- https://quarkus.io/guides/class-loading-reference
- https://quarkus.io/guides/writing-native-applications-tips

---

# Rutas especializadas futuras

Estos temas pueden incorporarse después del tronco principal cuando exista una aplicación o problema que justifique estudiarlos:

- GraphQL;
- gRPC;
- WebSockets Next;
- MongoDB;
- Redis;
- Hibernate Reactive;
- REST Data with Panache;
- Qute;
- WebAuthn;
- OpenShift;
- Kubernetes Operators;
- compatibilidad con APIs de Spring;
- LangChain4j y capacidades de IA;
- otras extensiones relevantes.

La intención es evitar convertir el curso en un catálogo de extensiones.

---

# Evolución resumida del Issue Tracker

## Etapa 1 — Esqueleto

Quarkus, configuración, CDI básico y separación progresiva de responsabilidades.

## Etapa 2 — API en memoria

Projects e Issues mediante REST, DTO, validación y manejo de errores.

## Etapa 3 — Pruebas

La aplicación comienza a incorporar tests como parte habitual del desarrollo.

## Etapa 4 — Persistencia

PostgreSQL, Hibernate ORM, Panache, transacciones y Flyway reemplazan la implementación en memoria.

## Etapa 5 — Integraciones

La aplicación consume servicios externos de forma controlada y resiliente.

## Etapa 6 — Seguridad

Aparecen usuarios, autenticación, roles, miembros de proyectos y permisos contextuales.

## Etapa 7 — Eventos

El Issue Tracker publica y consume eventos mediante Quarkus Messaging y Kafka.

## Etapa 8 — Ejecución y rendimiento

Se entienden blocking/non-blocking, Mutiny, virtual threads, cache y tareas programadas.

## Etapa 9 — CDI avanzado

Se consolidan qualifiers, producers, interceptores y eventos CDI a partir de necesidades ya conocidas.

## Etapa 10 — Observabilidad

Logging, health, métricas y trazabilidad permiten operar la aplicación.

## Etapa 11 — Producción

Empaquetado, contenedores, configuración externa, Kubernetes y native image cierran el recorrido.

---

# Infraestructura local

La estrategia preferida será:

```text
Aplicación Quarkus
       │
       ├── dev
       │    └── Dev Services
       │           └── Podman
       │
       ├── test
       │    └── Dev Services / Quarkus Test Resources
       │
       └── producción
            └── servicios reales o administrados
```

Cuando Dev Services no sea suficiente o necesitemos controlar un conjunto de servicios relacionado, se podrá estudiar **Compose Dev Services**.

**Fuentes oficiales:**
- https://quarkus.io/guides/dev-services
- https://quarkus.io/guides/compose-dev-services
- https://quarkus.io/guides/podman

---

# Criterio para modificar este roadmap

Una sesión puede moverse, dividirse, combinarse o eliminarse cuando:

- una versión actual de Quarkus cambie significativamente la tecnología;
- la documentación oficial recomiende un enfoque diferente;
- la experimentación muestre que el orden no es pedagógico;
- el Issue Tracker necesite un concepto previo que todavía no haya sido explicado;
- un tema resulte demasiado amplio para una sola sesión;
- un tema no aporte suficiente valor al tronco principal.

Antes de convertir una decisión todavía inmadura en estructura definitiva del repositorio, debe discutirse y experimentarse.

El roadmap existe para servir al aprendizaje, no al revés.
