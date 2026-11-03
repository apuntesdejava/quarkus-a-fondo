# Sesión 03 — Dev Mode y ciclo de desarrollo

## Objetivo de aprendizaje

Al terminar la sesión, el estudiante debería poder utilizar Dev Mode como ciclo habitual de desarrollo en Quarkus, comprender qué ocurre cuando modifica código mientras la aplicación está en ejecución, reconocer el papel del live reload y distinguir claramente este modo de trabajo de la ejecución de una aplicación empaquetada.

La sesión se concentra en la experiencia de desarrollo. No estudia todavía Dev UI, configuración avanzada, perfiles ni testing continuo; esos temas tendrán sus propias sesiones.

## Entorno base

La sesión utiliza el baseline actual del curso:

- JDK 25;
- Apache Maven 3.9.16;
- Git;
- Quarkus 3.40.1.1.

Maven continúa siendo la herramienta principal del curso. La Quarkus CLI no forma parte de los requisitos.

## El problema que resuelve Dev Mode

En un ciclo de desarrollo tradicional, un cambio puede obligar a repetir varias acciones:

```text
editar código
     │
     ▼
compilar
     │
     ▼
reiniciar la aplicación
     │
     ▼
volver a probar
```

Cuando estos pasos se repiten constantemente, el tiempo de espera interrumpe el flujo de trabajo.

Quarkus incorpora un modo de desarrollo que reduce ese ciclo. La aplicación permanece ejecutándose mientras Quarkus supervisa cambios relevantes del proyecto, recompila cuando corresponde y vuelve a desplegar la aplicación para que el siguiente intento utilice el código actualizado.

Con Maven se inicia mediante:

```powershell
.\mvnw.cmd quarkus:dev
```

No es necesario ejecutar previamente `mvn compile` o `mvn package` en un proyecto Quarkus normal. El propio goal `quarkus:dev` prepara el proyecto y mantiene un proceso de compilación en segundo plano.

Un proyecto concreto puede necesitar pasos adicionales si su build contiene plugins que generan código u otros artefactos durante fases anteriores del lifecycle de Maven. Ese requisito pertenece al build de ese proyecto, no a Dev Mode como regla general.

## Qué ocurre al ejecutar `quarkus:dev`

`quarkus:dev` es un goal del Quarkus Maven Plugin destinado específicamente al desarrollo.

Al iniciar la aplicación aparecen mensajes como:

```text
Profile dev activated. Live Coding activated.
```

y, por defecto, Quarkus habilita también debugging en el puerto `5005`.

Dev Mode mantiene infraestructura adicional para facilitar tareas propias del desarrollo, entre ellas la detección de cambios y el live reload. Esta infraestructura no debe confundirse con la forma en que se ejecuta una aplicación empaquetada para producción.

La salida también puede mostrar:

```text
Tests paused
```

Quarkus integra continuous testing con Dev Mode. En esta etapa del curso únicamente reconocemos que esa capacidad existe; se estudiará formalmente cuando lleguemos al bloque de testing.

## Live reload

Quarkus permite modificar fuentes, recursos y configuración mientras Dev Mode está activo.

En el flujo HTTP habitual, guardar un archivo no implica necesariamente que se vea actividad inmediata en la consola. Cuando llega una nueva petición, Quarkus comprueba el workspace, detecta los cambios, recompila lo necesario y realiza el redeploy antes de atender esa petición.

El ciclo observado durante esta sesión puede resumirse así:

```text
modificar y guardar
        │
        ▼
llega una nueva petición
        │
        ▼
Quarkus detecta cambios
        │
        ▼
recompila
        │
        ▼
reinicia / recarga la aplicación
        │
        ▼
atiende la petición
con la nueva versión
```

Durante el experimento, después de modificar `GreetingResource`, la consola informó:

```text
Restarting quarkus due to changes in GreetingResource.class.
```

Después detuvo e inició nuevamente la aplicación y mostró el tiempo total empleado por el live reload.

Esto permite precisar una idea importante: live reload no significa necesariamente que una clase sea sustituida en memoria sin reiniciar nada. Quarkus puede reiniciar la aplicación cuando el cambio lo requiere.

Quarkus dispone también de instrumentation-based reload para determinados cambios compatibles con redefinición de clases, pero esa capacidad no forma parte del experimento de esta sesión.

## Qué tipos de cambios puede detectar

Dev Mode puede reflejar cambios en:

- código fuente;
- archivos de recursos;
- configuración, incluido `application.properties`.

Quarkus también puede detectar cambios relevantes en el `pom.xml` y reiniciar el proceso Maven cuando sea necesario, por ejemplo después de cambios que afectan las dependencias o extensiones.

En esta sesión experimentamos únicamente con código Java. Los cambios de configuración y otras capacidades se utilizarán en sus sesiones correspondientes para no adelantar conceptos.

## Errores de compilación durante Dev Mode

Dev Mode resulta especialmente útil cuando el cambio todavía no compila.

Durante la demo eliminamos deliberadamente un `;` del recurso. La siguiente petición provocó la recompilación y la consola informó:

```text
Compilation Failed:
...
error: ';' expected
```

La aplicación no necesitó ser detenida ni `quarkus:dev` tuvo que ejecutarse nuevamente.

Después de corregir el código y realizar otra petición, Quarkus detectó el nuevo cambio, volvió a compilar y la aplicación quedó operativa otra vez.

El ciclo fue:

```text
cambio inválido
      │
      ▼
fallo de compilación
      │
      ▼
corregir código
      │
      ▼
nueva petición
      │
      ▼
recompilación
      │
      ▼
aplicación operativa
```

Las páginas de error detalladas que Quarkus puede devolver en este escenario forman parte de las ayudas disponibles durante Dev Mode y no deben considerarse comportamiento de producción.

## Consultar el endpoint desde PowerShell

Para una comprobación rápida del cuerpo de la respuesta utilizamos:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

El resultado inicial es:

```text
Hello from Quarkus REST
```

Cuando interesa observar más detalles de la respuesta HTTP puede utilizarse:

```powershell
Invoke-WebRequest http://localhost:8080/hello
```

La salida permite ver información como:

```text
StatusCode        : 200
StatusDescription : OK
Content           : Hello from Quarkus REST
```

y cabeceras como `Content-Type`.

En esta etapa del curso la línea de comandos es suficiente. Herramientas dedicadas para trabajar con APIs serán más útiles cuando los requests sean más complejos.

## Dev Mode no es ejecución de producción

Dev Mode está diseñado para desarrollar, no para ejecutar una aplicación en producción.

La diferencia no se limita a que uno tenga live reload y el otro no. Quarkus utiliza infraestructura específica en Dev Mode para permitir ese ciclo de desarrollo. La aplicación empaquetada utiliza una organización distinta orientada a la ejecución normal.

Durante Dev Mode vemos:

```text
Profile dev activated. Live Coding activated.
```

Después de empaquetar y ejecutar la aplicación:

```powershell
.\mvnw.cmd package
java -jar .\target\quarkus-app\quarkus-run.jar
```

la salida muestra:

```text
Profile prod activated.
```

En esa ejecución no aparecen `Live Coding activated` ni la consola interactiva propia de Dev Mode.

La diferencia se hace especialmente visible al modificar el fuente mientras el JAR está ejecutándose.

El artefacto fue construido cuando `GreetingResource` contenía:

```java
return "Hello from Quarkus REST";
```

Después de iniciar el JAR se modificó el archivo fuente a:

```java
return "Hello from Dev Mode";
```

La aplicación empaquetada continuó respondiendo:

```text
Hello from Quarkus REST
```

El proceso en ejecución no observa los archivos fuente ni recompila automáticamente la aplicación.

```text
Dev Mode

source cambia
     │
     ▼
Quarkus detecta
     │
     ▼
recompila y recarga
     │
     ▼
respuesta actualizada
```

```text
Aplicación empaquetada

source cambia
     │
     ▼
el proceso no cambia
     │
     ▼
continúa ejecutando
el artefacto construido
```

Por esta razón no debemos esperar que modificar código o configuración en el servidor produzca en producción el mismo comportamiento que observamos durante Dev Mode.

Quarkus indica expresamente que Dev Mode no debe utilizarse en producción. Además de las diferencias arquitectónicas, puede habilitar información, herramientas y operaciones destinadas exclusivamente al desarrollo.

## `compile`, `package` y `quarkus:dev`

Estos comandos responden a objetivos distintos.

### `mvn compile`

Ejecuta las fases necesarias hasta compilación y produce las clases compiladas del proyecto.

### `mvn package`

Recorre el lifecycle hasta `package`. En el packaging `quarkus` generado actualmente, esto incluye compilación, compilación de tests, ejecución de tests y finalmente `quarkus:build`.

En la demo observamos:

```text
compiler:compile
...
surefire:test
...
quarkus:build
...
BUILD SUCCESS
```

El resultado es una aplicación empaquetada que puede ejecutarse independientemente de los fuentes.

### `mvn quarkus:dev`

Inicia un entorno interactivo de desarrollo con compilación en segundo plano y live reload.

No debe entenderse como una forma abreviada de ejecutar continuamente `mvn package`.

```text
compile
   │
   ▼
obtener clases compiladas

package
   │
   ▼
construir la aplicación empaquetada

quarkus:dev
   │
   ▼
trabajar en un entorno de desarrollo
con compilación y live reload
```

## Demo aislada: `demos/03-dev-mode/`

La demo de esta sesión vive en:

```text
demos/03-dev-mode/
```

Su código publicado permanece en el estado base generado por Quarkus. Los cambios utilizados para experimentar con Dev Mode son deliberadamente temporales.

El recorrido de la demo es:

```text
iniciar quarkus:dev
        │
        ▼
consultar /hello
        │
        ▼
modificar el texto de respuesta
        │
        ▼
consultar nuevamente
        │
        ▼
observar recompilación y reload
        │
        ▼
introducir un error de compilación
        │
        ▼
observar el error
        │
        ▼
corregirlo
        │
        ▼
comprobar la recuperación
        │
        ▼
empaquetar la aplicación
        │
        ▼
ejecutar el JAR
        │
        ▼
modificar el source
        │
        ▼
comprobar que el JAR no cambia
```

No se introduce funcionalidad REST nueva. El recurso generado sirve únicamente como una forma sencilla de observar el ciclo de desarrollo.

## Aplicación transversal: Issue Tracker

El Issue Tracker continúa desde el estado final de la sesión 02.

En esta sesión tampoco incorporamos nueva funcionalidad de negocio. Utilizamos el recurso existente para aplicar el mismo flujo de desarrollo sobre la aplicación transversal.

Con Dev Mode activo:

```powershell
.\mvnw.cmd quarkus:dev
```

consultamos:

```powershell
Invoke-RestMethod http://localhost:8080/hello
```

y obtenemos:

```text
Hello from Quarkus REST
```

Temporalmente cambiamos la respuesta por:

```java
return "Issue Tracker is running";
```

La siguiente petición provocó el live reload y devolvió:

```text
Issue Tracker is running
```

Después del experimento el recurso se devuelve a su estado original.

Esto evita modificar los tests generados o introducir conceptos de testing antes de su sesión correspondiente. La evolución de esta sesión en el Issue Tracker es de proceso: desde este punto, `quarkus:dev` pasa a ser la forma habitual de ejecutar la aplicación mientras desarrollamos.

## Verificaciones manuales

Para esta sesión son suficientes las siguientes comprobaciones:

1. Dev Mode inicia correctamente.
2. `/hello` responde.
3. Un cambio Java es detectado y reflejado después de una nueva petición.
4. Un error de compilación queda visible sin tener que relanzar Dev Mode.
5. Después de corregir el error, la aplicación vuelve a funcionar.
6. La aplicación empaquetada continúa ejecutando el código con el que fue construida aunque cambien los fuentes.
7. El mismo ciclo puede utilizarse sobre el Issue Tracker.

No incorporamos nuevas herramientas de testing en esta sesión. Los tests generados permanecen en los proyectos, pero su funcionamiento se estudiará formalmente más adelante.

## Ideas equivocadas frecuentes

### «Guardar un archivo cambia inmediatamente la aplicación»

No necesariamente. En el flujo HTTP habitual, una nueva petición puede ser el evento que provoca el escaneo del workspace y el posterior reload.

### «Live reload siempre sustituye la clase sin reiniciar»

No. Quarkus puede reiniciar la aplicación cuando el cambio lo requiere. La consola muestra claramente cuándo esto sucede.

### «Hay que ejecutar `mvn compile` antes de `quarkus:dev`»

No como regla general. Dev Mode mantiene su propio ciclo de compilación.

Proyectos con plugins o generación de código personalizada pueden necesitar pasos adicionales debido a su build concreto.

### «Dev Mode es simplemente ejecutar el JAR con vigilancia de archivos»

No. Es un modo de ejecución específico con infraestructura adicional destinada al desarrollo.

### «Si funciona en Dev Mode, en producción también puedo modificar archivos y esperar que se recarguen»

No. La aplicación empaquetada no utiliza el mecanismo de live reload de Dev Mode.

### «Dev Mode puede utilizarse en producción»

No. Quarkus lo considera un entorno exclusivamente de desarrollo y existen diferencias arquitectónicas, funcionales y de seguridad respecto a una aplicación normal.

## Resultado final de la sesión

Al finalizar:

- existe `demos/03-dev-mode/`;
- sabemos iniciar una aplicación con `quarkus:dev`;
- entendemos el ciclo de detección, recompilación y live reload;
- comprobamos cómo Dev Mode se recupera después de corregir un error de compilación;
- distinguimos Dev Mode de `compile` y `package`;
- verificamos que una aplicación empaquetada no observa cambios realizados posteriormente en los fuentes;
- utilizamos el mismo ciclo sobre el Issue Tracker sin introducir todavía nueva funcionalidad REST;
- dejamos testing continuo, Dev UI y configuración para sus sesiones correspondientes.

## Conexión con la sesión 04

Dev Mode proporciona el entorno interactivo que utilizaremos continuamente mientras desarrollamos.

La siguiente sesión se concentra en una herramienta disponible precisamente dentro de ese entorno:

**Sesión 04 — Dev UI.**

## Fuentes oficiales

Fuentes principales utilizadas para preparar y verificar esta sesión:

- Quarkus — Quarkus and Maven: https://quarkus.io/guides/maven-tooling
- Quarkus — Quarkus Maven plugin: https://quarkus.io/guides/quarkus-maven-plugin
- Quarkus — How dev mode differs from a production application: https://quarkus.io/guides/dev-mode-differences
- Quarkus — Continuous testing: https://quarkus.io/guides/continuous-testing
