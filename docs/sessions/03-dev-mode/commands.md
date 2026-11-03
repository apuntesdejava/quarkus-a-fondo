# Comandos — Sesión 03

## Demo aislada

Desde la raíz del repositorio:

```powershell
Set-Location .\demos\03-dev-mode
```

Iniciar Dev Mode:

```powershell
.\mvnw.cmd quarkus:dev
```

En otra terminal, comprobar el endpoint:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

Mostrar detalles de la respuesta HTTP:

```powershell
Invoke-WebRequest http://localhost:8080/hello
```

Después de modificar `GreetingResource.java`, volver a comprobar:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

Después de introducir y corregir un error de compilación, comprobar nuevamente:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

Detener Dev Mode desde su consola interactiva:

```text
q
```

Empaquetar la aplicación:

```powershell
.\mvnw.cmd package
```

Ejecutar la aplicación empaquetada:

```powershell
java -jar .\target\quarkus-app\quarkus-run.jar
```

En otra terminal, comprobar la respuesta:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

## Issue Tracker

Desde la raíz del repositorio:

```powershell
Set-Location .\issue-tracker
```

Iniciar Dev Mode:

```powershell
.\mvnw.cmd quarkus:dev
```

En otra terminal, comprobar el endpoint existente:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

Después de modificar temporalmente `GreetingResource.java`, volver a comprobar:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

Detener Dev Mode desde su consola interactiva:

```text
q
```

## Verificación final

Desde la raíz del repositorio, comprobar que el Issue Tracker sigue pasando sus tests:

```powershell
.\issue-tracker\mvnw.cmd -f .\issue-tracker\pom.xml test
```
