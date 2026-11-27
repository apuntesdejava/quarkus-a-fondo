# Comandos - Sesión 04.5: Actualizamos Quarkus a Fondo a Quarkus 4

## Demo aislada

La demo se encuentra en:

`demos/04-5-quarkus-4-migration/`

Para reproducir la actualización, se debe partir del estado de la demo con Quarkus `3.40.1.1`, disponible en el historial de Git. El estado final publicado utiliza Quarkus `4.0.1`.

### Actualizar Quarkus

Desde el directorio de la demo:

```powershell
mvn io.quarkus.platform:quarkus-maven-plugin:4.0.1:update -N
```

Cuando el actualizador solicite confirmación para aplicar las recetas de OpenRewrite, responder `y`.

### Revisar los cambios

```powershell
git diff --check
git diff -- pom.xml
```

### Compilar y verificar

```powershell
mvn clean verify
```

### Ejecutar en Dev Mode

```powershell
mvn quarkus:dev
```

Desde otra terminal PowerShell:

```powershell
Invoke-WebRequest http://localhost:8080/hello | Select-Object StatusCode, Content
```

Resultado esperado:

```text
StatusCode Content
---------- -------
       200 Hello from Quarkus REST
```

## Issue Tracker

El Issue Tracker se encuentra en:

`issue-tracker/`

Su actualización a Quarkus 4.0.1 ya forma parte del estado final de esta sesión.

Para comprobar su funcionamiento, desde ese directorio:

```powershell
mvn clean verify
```

Ejecutar en Dev Mode:

```powershell
mvn quarkus:dev
```

Desde otra terminal PowerShell:

```powershell
Invoke-WebRequest http://localhost:8080/hello | Select-Object StatusCode, Content
```

Dev UI estará disponible en:

`http://localhost:8080/q/dev-ui`

Las dos aplicaciones utilizan el puerto 8080 por defecto. Deben ejecutarse por separado.