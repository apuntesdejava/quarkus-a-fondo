# 01-getting-started

Demo aislada de la sesión 01 de **Quarkus a Fondo**.

Su objetivo es comprobar que podemos crear, ejecutar y verificar una aplicación Quarkus mínima antes de estudiar en detalle la estructura del proyecto, Dev Mode, Dev UI, configuración, testing o empaquetado.

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

## Material de la sesión

La explicación completa, comandos y diapositivas están en:

`docs/sessions/01-que-es-quarkus/`

Video:

https://youtu.be/4he5y7Fr2TU?si=ou4iMOlWNRKypFGy
