# Quarkus a Fondo

Curso gratuito, público y práctico sobre Quarkus, construido a partir de la documentación oficial, experimentación reproducible y una aplicación transversal que evoluciona sesión a sesión.

La idea central del curso es:

> documentación oficial → comprensión → experimentación → demo aislada → aplicación real → video → código público

## Cómo está organizado

Cada sesión trabaja el mismo concepto de dos maneras:

1. **Demo aislada:** una aplicación pequeña dedicada únicamente al tema de la sesión.
2. **Issue Tracker:** aplicación transversal que incorpora progresivamente lo aprendido.

El repositorio separa documentación, demos y aplicación transversal:

```text
quarkus-a-fondo/
├── docs/
│   ├── roadmap.md
│   └── sessions/
├── demos/
└── issue-tracker/
```

Maven es la herramienta principal del curso. La Quarkus CLI no es un requisito.

## Sesiones

| # | Tema | Video | Material | Estado |
|---|---|---|---|---|
| 01 | Qué es Quarkus y qué problema intenta resolver | [YouTube](https://youtu.be/4he5y7Fr2TU?si=ou4iMOlWNRKypFGy) | [Sesión 01](docs/sessions/01-que-es-quarkus/) | En preparación |
| 02 | Crear un proyecto: Maven, extensiones y estructura | — | — | Planificada |
| 03 | Dev Mode y ciclo de desarrollo | — | — | Planificada |
| 04 | Dev UI | — | — | Planificada |
| 05 | Configuración con SmallRye Config | — | — | Planificada |
| 06 | Perfiles y fuentes de configuración | — | — | Planificada |
| 07 | CDI esencial: beans, scopes e inyección | — | — | Planificada |

El recorrido completo, incluyendo persistencia, testing, integraciones, seguridad, messaging, observabilidad, producción y la ruta avanzada, está en el [roadmap](docs/roadmap.md).

## Recuperar el estado de una sesión

Las sesiones publicadas se conservarán mediante tags:

```bash
git checkout session-01
```

Cada tag representa el estado completo del repositorio al finalizar esa sesión.

## Material de cada sesión

La documentación pública de una sesión vive en:

```text
docs/sessions/<nn>-<slug>/
├── README.md
├── commands.md
└── slides.md
```

- `README.md`: explicación técnica y pedagógica.
- `commands.md`: comandos reproducibles utilizados en la sesión.
- `slides.md`: presentación en Marp.

Los guiones, runbooks y notas internas de producción no forman parte de este repositorio público.

## Requisitos base

El baseline inicial del curso es:

- JDK 25;
- Apache Maven 3.9.16;
- Git.

Las versiones pueden evolucionar junto con el curso. Cuando una sesión dependa de una versión concreta, se indicará en su material.

## Fuentes

La documentación oficial actual de Quarkus es la fuente principal. Cuando corresponde, el curso complementa con documentación oficial de proyectos relacionados como Jakarta EE, MicroProfile, Hibernate, SmallRye o Keycloak.

Este proyecto no pretende sustituir la documentación oficial, sino enseñar a entenderla, utilizarla y convertirla en conocimiento práctico.
