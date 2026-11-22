# Sesión 04 — Dev UI

## Objetivo de aprendizaje

Al terminar la sesión, el estudiante debería poder reconocer Dev UI como una herramienta propia del entorno de desarrollo de Quarkus, acceder a ella, utilizarla para explorar las capacidades presentes en una aplicación y distinguirla de la aplicación que se ejecuta normalmente después de ser empaquetada.

Esta sesión es una introducción. Dev UI ofrece bastante más de lo que necesitamos estudiar ahora y su contenido crecerá a medida que incorporemos nuevas capacidades al curso. Volveremos a ella en sesiones posteriores cuando estudiemos configuración, testing, persistencia, Dev Services y otras áreas de Quarkus.

Dev UI se presenta aquí, pero se aprende realmente a lo largo del curso.

## Video

[Ver la sesión 04 en YouTube](https://youtu.be/BojvQvoStDs)

## Entorno base

La sesión utiliza el baseline actual del curso:

* JDK 25;
* Apache Maven 3.9.16;
* Git;
* Quarkus 3.40.1.1.

Maven continúa siendo la herramienta principal del curso. La Quarkus CLI no forma parte de los requisitos.

## Dev UI dentro de Development Mode

En la sesión anterior estudiamos Dev Mode y el ciclo de desarrollo que ofrece Quarkus. Dev UI forma parte de esa misma experiencia.

Iniciamos la aplicación con:

```powershell
.\\mvnw.cmd quarkus:dev
```

y accedemos a:

```text
http://localhost:8080/q/dev-ui
```

Quarkus también permite abrir Dev UI desde la consola interactiva de Dev Mode pulsando `d`.

Dev UI es una interfaz destinada al desarrollo que permite explorar, depurar e interactuar con la aplicación mientras está ejecutándose. No es una consola administrativa destinada a acompañar una aplicación desplegada en producción.

Al ejecutar `quarkus:dev`, Quarkus entra en Development Mode y activa automáticamente el perfil `dev`. Son conceptos relacionados, pero no equivalentes: Dev UI forma parte de Development Mode; no aparece simplemente por la existencia de un perfil llamado `dev`.

La relación puede resumirse así:

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

## Una ventana a la aplicación

Dev UI no presenta únicamente una colección fija de pantallas. Su contenido refleja las capacidades disponibles en la aplicación y puede ampliarse mediante las extensiones de Quarkus.

En la demo de esta sesión la página de extensiones muestra, entre otras:

* REST;
* ArC;
* Eclipse Vert.x;
* Mutiny;
* SmallRye Context Propagation;
* JSON-P.

No necesitamos comprender todavía cada una de ellas. Algunas llegarán formalmente mucho más adelante.

Lo importante en esta sesión es observar que la aplicación está compuesta por más piezas de las que necesariamente hemos escrito de forma explícita y que Quarkus puede mostrarnos información relacionada con esas capacidades desde un único lugar.

A medida que añadamos extensiones y funcionalidades, Dev UI también irá cambiando.

## Extensiones

La página **Extensions** muestra las extensiones utilizadas por la aplicación mediante tarjetas.

Estas tarjetas pueden proporcionar, según la extensión:

* información sobre la capacidad instalada;
* enlaces a su guía oficial;
* acceso a configuración relacionada;
* páginas o herramientas específicas;
* datos obtenidos durante el desarrollo.

Esto explica por qué dos aplicaciones Quarkus pueden mostrar una Dev UI diferente. El contenido depende de las capacidades y extensiones presentes en cada aplicación.

En esta sesión no recorremos cada tarjeta. Utilizamos esta vista para entender que Dev UI es extensible y que volveremos a encontrar nuevas herramientas aquí conforme avance el curso.

## Endpoints

La página **Endpoints** permite observar los endpoints HTTP conocidos por la aplicación.

En nuestra demo aparece:

```text
/hello
```

que corresponde al recurso existente en el proyecto.

También aparecen rutas bajo `/q`, relacionadas con herramientas y capacidades disponibles durante el desarrollo, por ejemplo la propia Dev UI.

Esta pantalla resulta útil para comprobar qué superficie HTTP reconoce Quarkus sin necesidad de estudiar todavía cómo se implementa cada endpoint.

Jakarta REST tendrá su propia parte del curso. En esta sesión `/hello` sirve únicamente como un elemento conocido que podemos localizar desde Dev UI.

## Workspace

La página **Workspace** permite navegar los archivos que forman parte del proyecto.

Desde allí podemos localizar elementos conocidos como:

```text
pom.xml
README.md
src/
```

y abrir archivos como:

```text
GreetingResource.java
application.properties
```

Esta capacidad no pretende sustituir al IDE. Su utilidad aquí es ofrecer otra perspectiva del proyecto desde el propio entorno de desarrollo de Quarkus y permitir que determinadas extensiones añadan acciones relacionadas con los archivos del workspace.

## Configuration: una pantalla que volveremos a visitar

La página **Configuration** muestra las propiedades de configuración conocidas por la aplicación, sus valores actuales, valores por defecto y documentación asociada.

En una aplicación aparentemente pequeña pueden aparecer más de mil propiedades.

No intentaremos estudiarlas en esta sesión.

Todavía no hemos explicado cómo funciona la configuración de Quarkus, cómo se resuelven los valores, cómo se definen propiedades propias ni cómo intervienen los perfiles. Esa será precisamente la siguiente etapa del curso.

Por ahora basta con saber que esta información existe y que Dev UI nos permitirá explorarla cuando tengamos el conocimiento necesario para interpretarla.

## Hay más, pero todavía no necesitamos aprenderlo

Dev UI contiene otras áreas que iremos utilizando cuando aparezca una necesidad real.

Entre ellas se encuentran:

* **Continuous Testing**, para controlar y observar las pruebas continuas;
* **Dev Services**, para mostrar servicios iniciados automáticamente durante desarrollo;
* **Build Metrics**, con información sobre el proceso interno de construcción de Quarkus;
* **Dependencies**, para explorar dependencias de runtime y deployment;
* **Readme**, que representa el `README.md` del proyecto.

No es necesario aprender todas estas herramientas ahora.

Por ejemplo, la página de Dev Services está vacía en esta demo porque todavía no utilizamos ninguna capacidad que necesite iniciar uno de esos servicios. Cuando estudiemos tecnologías que sí los utilicen, volveremos a la misma pantalla y tendrá mucho más sentido.

Del mismo modo, Build Metrics expone conceptos internos como build steps y build items. Son parte de cómo Quarkus y sus extensiones realizan trabajo durante la construcción, pero estudiarlos aquí desviaría la sesión hacia internals que todavía no necesitamos.

La existencia de estas pantallas es suficiente por ahora.

## Dev UI no forma parte de la ejecución normal

La forma más clara de comprobar la naturaleza de Dev UI es comparar Development Mode con una aplicación empaquetada.

Con Dev Mode activo:

```powershell
.\\mvnw.cmd quarkus:dev
```

esta ruta está disponible:

```text
http://localhost:8080/q/dev-ui
```

Después detenemos Dev Mode y empaquetamos la aplicación:

```powershell
.\\mvnw.cmd package
```

La ejecutamos normalmente:

```powershell
java -jar .\\target\\quarkus-app\\quarkus-run.jar
```

El endpoint de nuestra aplicación continúa disponible:

```text
http://localhost:8080/hello
```

pero al solicitar:

```text
http://localhost:8080/q/dev-ui
```

obtenemos:

```text
Resource not found
```

Esto demuestra que Dev UI pertenece al tooling de desarrollo de Quarkus y no a la superficie normal de la aplicación empaquetada.

Fuera de Development Mode y de las pruebas, Quarkus utiliza el perfil `prod` por defecto. La diferencia, sin embargo, no debe reducirse únicamente al nombre del perfil: Development Mode activa infraestructura y herramientas específicamente destinadas al desarrollo.

Quarkus indica además que Dev Mode no debe utilizarse en producción. Dev UI puede mostrar información sobre el estado de la aplicación y, dependiendo de las extensiones instaladas, ofrecer acciones capaces de modificarlo.

## Demo aislada: `demos/04-dev-ui/`

La demo de esta sesión vive en:

```text
demos/04-dev-ui/
```

La aplicación es deliberadamente pequeña.

Contiene las mismas capacidades mínimas que necesitamos para disponer de algo real que observar:

```text
REST
ArC
/hello
```

y define:

```properties
quarkus.application.name=04-dev-ui
```

para que la aplicación pueda identificarse claramente en Dev UI.

No añadimos extensiones únicamente para llenar la interfaz con más tarjetas. La intención es observar el estado real de una aplicación sencilla y dejar que Dev UI crezca de forma natural cuando el curso incorpore nuevas capacidades.

El recorrido principal de la demo es:

```text
mvn quarkus:dev
        │
        ▼
abrir /q/dev-ui
        │
        ▼
Extensions
        │
        ▼
Endpoints
        │
        ▼
Workspace
        │
        ▼
Configuration
        │
        ▼
detener Dev Mode
        │
        ▼
empaquetar y ejecutar el JAR
        │
        ▼
comprobar que /q/dev-ui
ya no está disponible
```

La demo conserva los tests generados, pero no se estudian todavía. Testing tendrá su propio bloque dentro del curso.

## Verificaciones manuales

Para esta sesión son suficientes las siguientes comprobaciones:

1. La aplicación inicia mediante `quarkus:dev`.
2. `/q/dev-ui` está disponible durante Development Mode.
3. Dev UI muestra las extensiones presentes en la aplicación.
4. Endpoints permite localizar `/hello`.
5. Workspace permite navegar el proyecto.
6. Configuration muestra las propiedades conocidas por Quarkus.
7. Después de empaquetar y ejecutar normalmente la aplicación, `/hello` continúa disponible pero `/q/dev-ui` no.

No necesitamos modificar funcionalidad de negocio para realizar estas verificaciones.

## Ideas equivocadas frecuentes

### «Dev UI es el panel de administración de mi aplicación»

No. Dev UI es tooling destinado al desarrollo. Puede mostrar información y ofrecer acciones sobre el estado de la aplicación, pero no debe tratarse como una consola administrativa de producción.

### «Dev UI siempre muestra las mismas herramientas»

No. Parte de su contenido depende de las extensiones y capacidades presentes en la aplicación.

### «Tengo que entender todo lo que aparece antes de continuar»

No. Varias áreas de Dev UI corresponden a conceptos que todavía no hemos estudiado. Volveremos a ellas cuando tengan contexto.

### «Dev UI aparece simplemente porque existe el perfil `dev`»

No es la forma adecuada de entenderlo. `quarkus:dev` inicia Development Mode, y Quarkus activa automáticamente el perfil `dev` dentro de ese modo. Dev UI forma parte de la experiencia de Development Mode.

### «Si necesito Dev UI en un servidor, puedo ejecutar la aplicación en Dev Mode»

No. Quarkus desaconseja explícitamente utilizar Dev Mode en producción. Está diseñado para facilitar el desarrollo y habilita capacidades que no deberían exponerse de esa manera en un entorno productivo.

## Resultado final de la sesión

Al finalizar:

* existe `demos/04-dev-ui/`;
* sabemos acceder a Dev UI desde Development Mode;
* entendemos que Dev UI refleja capacidades presentes en la aplicación;
* utilizamos Extensions, Endpoints y Workspace para inspeccionar una aplicación sencilla;
* reconocemos Configuration sin adelantar todavía su funcionamiento;
* comprobamos que Dev UI no está disponible al ejecutar normalmente la aplicación empaquetada;
* sabemos que otras áreas de Dev UI cobrarán sentido conforme avance el curso.

Esta sesión no pretende cerrar el tema.

A partir de ahora Dev UI será una herramienta recurrente: cuando una nueva capacidad de Quarkus aporte algo útil a esta interfaz, volveremos a ella y la estudiaremos dentro del contexto correspondiente.

## Conexión con la sesión 05

La página Configuration ya nos mostró que Quarkus conoce una gran cantidad de propiedades y que Dev UI puede ayudarnos a explorarlas.

La siguiente sesión responderá las preguntas que dejamos abiertas aquí:

* ¿de dónde salen esos valores?;
* ¿cómo configuramos una aplicación Quarkus?;
* ¿qué valores tienen prioridad?;
* ¿cómo intervienen los perfiles?;
* ¿cómo podemos definir configuración propia?

**Sesión 05 — Configuración.**

## Fuentes oficiales

Fuentes principales utilizadas para preparar y verificar esta sesión:

* Quarkus — Dev UI: https://quarkus.io/guides/dev-ui/
* Quarkus — How dev mode differs from a production application: https://quarkus.io/guides/dev-mode-differences
* Quarkus — Configuration Reference Guide: https://quarkus.io/guides/config-reference
* Quarkus — Creating your first application: https://quarkus.io/guides/getting-started
