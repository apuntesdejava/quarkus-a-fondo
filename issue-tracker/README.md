# Issue Tracker

Aplicación transversal del curso **Quarkus a Fondo**.

En la sesión 01 su estado es deliberadamente mínimo: el proyecto existe, puede ejecutarse y expone únicamente el endpoint generado durante la creación inicial.

La aplicación irá incorporando funcionalidad progresivamente a medida que el curso estudie nuevos conceptos. En esta etapa no contiene todavía Projects, Issues, persistencia, seguridad, usuarios ni separación por capas.

## Ejecutar

Desde esta carpeta:

```powershell
mvn quarkus:dev
```

Con la aplicación en ejecución, desde otra terminal:

```powershell
Invoke-WebRequest -Uri "http://localhost:8080/hello" | Select-Object -ExpandProperty Content
```

Resultado esperado:

```text
Hello from Quarkus REST
```

## Evolución

El estado del Issue Tracker correspondiente a cada sesión publicada se podrá recuperar mediante tags del repositorio, por ejemplo:

```bash
git checkout session-01
```

## Material de la sesión 01

La explicación completa, comandos y diapositivas están en:

`docs/sessions/01-que-es-quarkus/`

Video:

https://youtu.be/4he5y7Fr2TU?si=ou4iMOlWNRKypFGy
