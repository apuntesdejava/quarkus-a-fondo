# Comandos — Sesión 04: Dev UI

Ejecutar la demo en Development Mode:

```powershell
cd demos/04-dev-ui
.\mvnw.cmd quarkus:dev
```

Dev UI estará disponible en:

```text
http://localhost:8080/q/dev-ui
```

Comprobar el endpoint de la aplicación:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

Después de detener Development Mode, empaquetar la aplicación:

```powershell
.\mvnw.cmd package
```

Ejecutar la aplicación empaquetada:

```powershell
java -jar .\target\quarkus-app\quarkus-run.jar
```

Comprobar que el endpoint de la aplicación continúa disponible:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

Durante esta ejecución, `/q/dev-ui` no está disponible.