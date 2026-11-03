---
marp: true
theme: default
paginate: true
---

# Quarkus a Fondo

## Sesión 03
### Dev Mode y ciclo de desarrollo

---

# El problema

Sin un ciclo de desarrollo asistido:

```text
editar código
     │
     ▼
compilar
     │
     ▼
reiniciar
     │
     ▼
probar otra vez
```

Repetir este ciclo constantemente interrumpe el flujo de trabajo.

---

# Dev Mode

```powershell
.\mvnw.cmd quarkus:dev
```

Inicia un entorno pensado específicamente para desarrollar:

```text
código fuente
     │
     ▼
quarkus:dev
     │
     ├── compilación
     ├── detección de cambios
     └── live reload
```

---

# Qué vemos al arrancar

```text
Profile dev activated.
Live Coding activated.
```

También aparecen:

```text
Listening for transport dt_socket at address: 5005
Tests paused
```

Hoy nos concentramos únicamente en Dev Mode y live reload.

---

# No necesita un compile previo

En un proyecto Quarkus normal:

```powershell
.\mvnw.cmd quarkus:dev
```

es suficiente.

Durante el arranque vimos:

```text
Invoking compiler:...:compile
Compiling ... to target\classes
```

Un build particular puede tener pasos previos adicionales si usa generación de código u otros plugins.

---

# Primera comprobación

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

```text
Hello from Quarkus REST
```

Para observar más detalles HTTP:

```powershell
Invoke-WebRequest http://localhost:8080/hello
```

---

# Cambiamos el código

```java
return "Hello from Dev Mode";
```

Guardamos el archivo.

**Todavía no reiniciamos Maven.**

---

# ¿Cuándo ocurre el reload?

```text
guardar cambio
      │
      ▼
nueva petición
      │
      ▼
Quarkus detecta cambios
      │
      ▼
recompila
      │
      ▼
restart / reload
      │
      ▼
nueva respuesta
```

---

# El log lo muestra

```text
Restarting quarkus due to changes in GreetingResource.class.
```

Después:

```text
03-dev-mode stopped ...
03-dev-mode ... started ...
Live reload total time: ...
```

Live reload no significa necesariamente sustituir una clase sin reiniciar la aplicación.

---

# ¿Y si el código no compila?

Provocamos un error deliberadamente:

```java
return "Hello from Dev Mode"
```

Quarkus informa:

```text
Compilation Failed:
error: ';' expected
```

No necesitamos detener y volver a lanzar `quarkus:dev`.

---

# Corregir y continuar

```text
error de compilación
        │
        ▼
corregir código
        │
        ▼
nueva petición
        │
        ▼
recompilar + reload
        │
        ▼
aplicación operativa
```

Dev Mode se recupera y seguimos trabajando.

---

# Dev Mode no es producción

Empaquetamos:

```powershell
.\mvnw.cmd package
```

Y ejecutamos:

```powershell
java -jar .\target\quarkus-app\quarkus-run.jar
```

Ahora aparece:

```text
Profile prod activated.
```

---

# Source vs artefacto empaquetado

El JAR fue construido con:

```java
return "Hello from Quarkus REST";
```

Mientras el JAR está ejecutándose, cambiamos el source a:

```java
return "Hello from Dev Mode";
```

La respuesta sigue siendo:

```text
Hello from Quarkus REST
```

---

# Dos ciclos diferentes

```text
DEV MODE

source cambia
     │
     ▼
Quarkus detecta
     │
     ▼
recompila / recarga
     │
     ▼
respuesta nueva
```

```text
APLICACIÓN EMPAQUETADA

source cambia
     │
     ▼
el proceso no cambia
     │
     ▼
sigue ejecutando
el artefacto construido
```

---

# compile, package y quarkus:dev

```text
mvn compile
     │
     ▼
clases compiladas
```

```text
mvn package
     │
     ▼
aplicación empaquetada
```

```text
mvn quarkus:dev
     │
     ▼
entorno interactivo de desarrollo
```

Tienen propósitos distintos.

---

# ¿Qué más puede detectar?

Dev Mode también puede reaccionar ante cambios en:

- recursos;
- configuración;
- `pom.xml` cuando corresponde.

No los demostramos hoy.

Los estudiaremos cuando cada concepto tenga su propia sesión.

---

# Continuous Testing

La consola muestra:

```text
Tests paused
```

Quarkus integra testing continuo con Dev Mode.

Lo dejamos pausado por ahora.

El testing se estudiará formalmente más adelante.

---

# Issue Tracker

No agregamos endpoints nuevos.

Utilizamos el recurso existente para repetir el mismo ciclo:

```text
quarkus:dev
     ↓
modificar temporalmente
     ↓
consultar /hello
     ↓
observar live reload
```

Después dejamos el proyecto en su estado original.

---

# Resultado

Ahora sabemos:

- iniciar Dev Mode;
- observar live reload;
- recuperarnos de errores de compilación;
- distinguir Dev Mode de una aplicación empaquetada;
- entender que `compile`, `package` y `quarkus:dev` cumplen funciones diferentes.

---

# Siguiente sesión

## Sesión 04
### Dev UI

Dev Mode ya nos da el entorno interactivo.

Ahora veremos una herramienta construida específicamente para trabajar dentro de ese entorno.
