# Sesión 04.5 - Actualizamos Quarkus a Fondo a Quarkus 4

## Objetivo de aprendizaje

Al terminar esta sesión, el estudiante debería poder utilizar las herramientas oficiales de actualización de Quarkus en una aplicación sencilla, interpretar las propuestas del actualizador, revisar los cambios y comprobar el funcionamiento que cubren las pruebas. Actualizamos las aplicaciones de este curso desde Quarkus **3.40.1.1** hasta **4.0.1**.

Esta es una sesión de transición entre Dev UI (sesión 04) y Configuración con SmallRye Config (sesión 05). Mantiene el código y el material de las sesiones publicadas en sus versiones originales; el resto del curso continuará sobre Quarkus 4.

## Alcance de la actualización

En esta sesión actualizamos **las aplicaciones que hemos desarrollado hasta ahora en Quarkus a Fondo**. El curso apenas está comenzando: la demo y el Issue Tracker contienen un recurso REST sencillo, utilizan las extensiones REST y ArC y cuentan con una prueba automatizada básica. Todavía no hemos desarrollado las partes de persistencia, serialización JSON, mensajería, seguridad ni integraciones externas.

En nuestros experimentos, el actualizador solo modificó dos propiedades del `pom.xml` de cada aplicación. **Eso no demuestra que cualquier proyecto basado en Quarkus 3 pueda migrarse a Quarkus 4 con esos mismos cambios.** Una aplicación con otras extensiones, dependencias y funcionalidades puede requerir cambios de código, configuración y pruebas adicionales.

Lo que aquí enseñamos es el procedimiento que seguimos para actualizar *nuestro curso* y las comprobaciones efectivamente realizadas. No pretendemos presentar una guía completa para migrar todas las aplicaciones Quarkus 3.

## Entorno de trabajo

Utilizamos el entorno habitual del curso: **JDK 25, Maven 3.9.16 y Git**. La actualización se realiza con el plugin Maven de Quarkus; no es necesario instalar ni utilizar Quarkus CLI.

La actualización se prueba sobre la demo aislada y el Issue Tracker, sin añadir nuevas tecnologías o funcionalidades. El alcance limitado de estas aplicaciones permite concentrarnos en el procedimiento y sus verificaciones.

## ¿Por qué una versión mayor requiere una revisión?

Una aplicación depende de más componentes que el número de versión declarado en su `pom.xml`. Al actualizar la plataforma pueden cambiar versiones de bibliotecas, APIs, propiedades de configuración y comportamientos en tiempo de ejecución. Quarkus 4 eleva el mínimo de Java a **21** y recomienda Java 25; también incorpora cambios importantes en Vert.x, Netty, Jackson, Hibernate ORM y especificaciones Jakarta.

No necesitamos explicar esas tecnologías en esta sesión. Lo importante es reconocer que una actualización mayor requiere **consultar la guía de migración**, además de ejecutar una herramienta de actualización.

La documentación oficial distingue los cambios que la herramienta puede aplicar automáticamente de aquellos que requieren revisión o intervención manual. El comando de actualización se considera **experimental** y utiliza recetas de OpenRewrite para transformar dependencias y, cuando corresponda, fuentes o configuración.

## Preparar una migración verificable

Antes de modificar una aplicación conviene establecer qué versión utiliza y comprobar su funcionamiento. Para esta sesión se verificaron ambas aplicaciones con Quarkus 3.40.1.1 antes de migrarlas: las compilaciones terminaron correctamente y cada proyecto superó su prueba `GreetingResourceTest`.

El procedimiento general que seguiremos es:

```text
identificar versión y dependencias
              │
              ▼
comprobar estado inicial
              │
              ▼
ejecutar el actualizador
              │
              ▼
inspeccionar el diff y la guía de migración
              │
              ▼
compilar y ejecutar pruebas
              │
              ▼
arrancar la aplicación y comprobar /hello
```

Una rama de trabajo o un estado identificable en Git permite separar este experimento del código publicado. En esta sesión utilizamos commits para conservar el punto de partida y el resultado.

## Demo aislada: `demos/04-5-quarkus-4-migration/`

La demo contiene `GreetingResource`, que responde en `/hello`, y una prueba automatizada de ese recurso. Partimos de la versión 3.40.1.1 y publicamos como resultado de la sesión la demo actualizada a Quarkus 4.0.1. El historial de Git permite consultar la evolución sin duplicar los proyectos.

En el `pom.xml` inicial encontramos:

```xml
<quarkus.platform.version>3.40.1.1</quarkus.platform.version>
<compiler-plugin.version>3.15.0</compiler-plugin.version>
<maven.compiler.release>25</maven.compiler.release>
```

El BOM importado desde `io.quarkus.platform:quarkus-bom` gestiona las versiones de las extensiones de Quarkus. Por ello, las dependencias `quarkus-rest` y `quarkus-arc` no declaran individualmente una versión.

### Actualizar con Maven

Desde la carpeta de la demo, partiendo del estado con Quarkus 3, ejecutamos:

```powershell
mvn io.quarkus.platform:quarkus-maven-plugin:4.0.1:update -N
```

`4.0.1` identifica la versión del plugin que ejecuta el objetivo de actualización. `-N` solicita a Maven que no realice una ejecución recursiva en módulos hijos. El objetivo analiza el proyecto, propone actualizaciones y solicita confirmación antes de aplicar las recetas generadas con OpenRewrite. En nuestra ejecución respondimos `y`.

El actualizador detectó Java 25, propuso actualizar el BOM de `3.40.1.1` a `4.0.1` e indicó que las extensiones REST y ArC continuarían sincronizadas con el BOM. También seleccionó dos recetas específicas para la transición a Quarkus 4 y registró la ejecución de OpenRewrite.

El mensaje `BUILD SUCCESS` que aparece al finalizar este comando **solo confirma que el proceso de actualización concluyó correctamente**. Todavía falta comprobar el código y la aplicación.

### Examinar los cambios

La herramienta modificó dos propiedades del `pom.xml`:

| Propiedad | Antes | Después |
|---|---|---|
| `quarkus.platform.version` | `3.40.1.1` | `4.0.1` |
| `compiler-plugin.version` | `3.15.0` | `3.16.0` |

La revisión se realizó con `git diff -- pom.xml` y `git diff --check`. No se encontraron cambios en los archivos Java de esta demo. Aunque OpenRewrite seleccionó recetas, eso no implica que cada receta vaya a transformar necesariamente una fuente: depende de los elementos presentes en el proyecto.

También se debe revisar el resto de los archivos modificados y consultar la guía oficial de migración para detectar tareas que no haya resuelto automáticamente la herramienta. La lista exacta de cambios depende de las extensiones y del código que utilice cada aplicación.

### Comprobar la demo migrada

Ejecutamos:

```powershell
mvn clean verify
```

La prueba `GreetingResourceTest` terminó con **1 prueba ejecutada, 0 fallos y 0 errores**. La compilación y el empaquetado también concluyeron con `BUILD SUCCESS`. Las pruebas de integración gestionadas por Failsafe estaban omitidas, por lo que no forman parte de esta verificación.

Después arrancamos la demo:

```powershell
mvn quarkus:dev
```

El registro confirmó `powered by Quarkus 4.0.1`, `Profile dev activated` y `Live Coding activated`. Desde otra terminal PowerShell comprobamos:

```powershell
Invoke-WebRequest http://localhost:8080/hello | Select-Object StatusCode, Content
```

Resultado observado:

```text
StatusCode Content
---------- -------
       200 Hello from Quarkus REST
```

Esta comprobación complementa la prueba automatizada: confirma que el endpoint responde durante la ejecución normal en Dev Mode.

## Aplicación transversal: Issue Tracker

Realizamos el mismo procedimiento sobre `issue-tracker/`. También en este caso OpenRewrite modificó únicamente la versión de la plataforma y la del Maven Compiler Plugin; las dependencias REST y ArC conservaron sus declaraciones.

Después de la actualización se comprobó lo siguiente:

| Comprobación | Resultado observado |
|---|---|
| `mvn clean verify` | `BUILD SUCCESS` |
| `GreetingResourceTest` | 1 prueba, 0 fallos, 0 errores |
| Arranque con `mvn quarkus:dev` | Quarkus 4.0.1, perfil `dev` |
| Dev UI | Disponible en `/q/dev-ui` |
| Consulta de `/hello` | HTTP 200, `Hello from Quarkus REST` |

No incorporamos nuevas funcionalidades al Issue Tracker: su evolución en esta sesión es **tecnológica**, porque cambia la versión del framework sobre la que continuaremos construyendo las capacidades del curso.

## Advertencias observadas

Durante las ejecuciones con Quarkus 4 aparecieron advertencias que no impidieron completar las comprobaciones.

Una indicaba que el artefacto no tenía un nombre de módulo definido y que Quarkus utilizaría uno calculado. Al inspeccionar el JAR del Issue Tracker comprobamos que no contenía `module-info.class` ni un atributo `Automatic-Module-Name` en su manifiesto; la herramienta `jar --describe-module` lo identificó como módulo automático con un nombre derivado del archivo. El nombre calculado por Quarkus era distinto. No fue necesario introducir JPMS ni modificar el proyecto para las verificaciones realizadas.

En Windows con Java 25 también apareció una advertencia de acceso nativo restringido originada en `org.aesh.terminal.tty.impl.WinConsoleNative`, una dependencia utilizada para la terminal. Es distinta de un error de compilación de la aplicación y merece seguimiento si cambia su comportamiento en versiones posteriores de Java o de la dependencia.

Por último, Quarkus solicitó consentimiento opcional para enviar estadísticas anónimas del proceso de construcción. Esa solicitud tampoco está relacionada con la compatibilidad de la aplicación.

En una migración real conviene **leer y clasificar las advertencias**, en vez de ignorarlas por sistema o silenciarlas sin comprender su origen.

## Interpretaciones equivocadas frecuentes

**"El actualizador terminó con `BUILD SUCCESS`; ya migré la aplicación".** Solo ha terminado el objetivo Maven de actualización. Es necesario inspeccionar el diff, revisar la guía de migración y comprobar el funcionamiento.

**"OpenRewrite siempre cambia código Java".** Las recetas se aplican cuando encuentran elementos correspondientes en el proyecto. En nuestra demo solo cambiaron dos propiedades de Maven.

**"Si el BOM cambió, debo fijar a mano todas las versiones de las extensiones".** Las extensiones administradas por el BOM reciben versiones coordinadas desde la plataforma. Conviene evitar versiones individuales innecesarias.

**"Una prueba que pasa demuestra compatibilidad completa".** Las pruebas verifican los casos que cubren. Aquí solo contamos con un recurso pequeño y una prueba. Aplicaciones con otras extensiones necesitan verificaciones adicionales.

**"Todas las advertencias nuevas requieren cambiar el código".** Primero hay que distinguir errores, advertencias de herramientas y cambios de comportamiento. En nuestros ejemplos las advertencias observadas no impidieron las verificaciones.

## Qué dejamos para sesiones posteriores

Quarkus 4 incorpora cambios que tendrán importancia cuando estudiemos las tecnologías correspondientes: Jackson 3 y serialización JSON, CDI 5, Jakarta REST 4, Hibernate ORM 8, entre otros. No adelantaremos su implementación aquí. La guía de migración será una referencia recurrente al incorporar esas capacidades.

## Resultado final

Al finalizar contamos con una demo aislada y un Issue Tracker sobre Quarkus **4.0.1**. En ambos proyectos verificamos la compilación, una prueba automatizada y la respuesta del endpoint `/hello` durante Dev Mode. La historia de Git conserva la versión previa de la demo y permite contrastar las modificaciones introducidas por la actualización. Las sesiones 01 a 04 y sus estados publicados siguen siendo referencias válidas de Quarkus 3.

En la **sesión 05 - Configuración con SmallRye Config** retomaremos la progresión del curso sobre Quarkus 4, utilizando el conocimiento de Dev UI adquirido en la sesión anterior.

## Fuentes oficiales

- [Quarkus - Update projects to the latest Quarkus version](https://quarkus.io/guides/update-quarkus/)
- [Quarkus - Migration Guide 4.0](https://github.com/quarkusio/quarkus/wiki/Migration-Guide-4.0)
- [Quarkus - Quarkus 4.0, our new major release](https://quarkus.io/blog/quarkus-4-0-released/)
- [Quarkus - Quarkus and Maven](https://quarkus.io/guides/maven-tooling)
