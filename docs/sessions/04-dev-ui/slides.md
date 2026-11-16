\---

marp: true
theme: default
paginate: true
---

# Sesión 04 — Dev UI

## Una ventana a la aplicación durante el desarrollo

**Quarkus a Fondo**

\---

# Dev UI dentro de Development Mode

```text
mvn quarkus:dev
        │
        ▼
Development Mode
        │
        ├── perfil dev
        ├── live reload
        ├── consola interactiva
        └── Dev UI
```

`http://localhost:8080/q/dev-ui`

\---

# Hoy la conocemos

## No intentaremos aprenderla completa

Dev UI puede mostrar:

* extensiones;
* endpoints;
* workspace;
* configuración;
* pruebas continuas;
* Dev Services;
* dependencias;
* métricas de construcción;
* herramientas aportadas por extensiones.

**Volveremos a ella durante todo el curso.**

\---

# Lo que muestra depende de la aplicación

En nuestra demo aparecen capacidades como:

```text
REST
ArC
Vert.x
Mutiny
SmallRye Context Propagation
JSON-P
```

Las extensiones pueden aportar información y herramientas propias a Dev UI.

**Dos aplicaciones Quarkus no tienen por qué mostrar la misma Dev UI.**

\---

# Tres vistas para empezar

## Extensions

Qué capacidades están presentes.

## Endpoints

Qué endpoints HTTP reconoce Quarkus.

## Workspace

Qué archivos forman parte del proyecto.

\---

# Configuration: la veremos después

Una aplicación pequeña ya expone una gran cantidad de propiedades conocidas por Quarkus.

```text
Configuration
     │
     ├── valores actuales
     ├── valores por defecto
     └── documentación
```

Todavía no necesitamos entenderlas.

**La siguiente sesión será Configuración.**

\---

# Dev UI pertenece al entorno de desarrollo

## Development Mode

```text
mvn quarkus:dev
/q/dev-ui  → disponible
```

## Aplicación empaquetada

```text
java -jar target/quarkus-app/quarkus-run.jar
/q/dev-ui  → Resource not found
```

Dev UI es tooling de desarrollo, no una consola de producción.

\---

# Esta sesión es solo el comienzo

```text
Dev UI
  │
  ├── configuración
  ├── testing
  ├── persistencia
  ├── Dev Services
  ├── OpenAPI
  ├── observabilidad
  └── ...
```

**Dev UI se presenta aquí, pero se aprende realmente a lo largo del curso.**

\---

# Siguiente sesión

## Sesión 05 — Configuración

Ahora que sabemos dónde mirar, toca entender:

* de dónde salen las propiedades;
* cómo se resuelven los valores;
* cómo intervienen los perfiles;
* cómo definir configuración propia.

