# Guion — Sesión 01: Qué es Quarkus y qué problema intenta resolver

## Propósito de la sesión

Esta primera sesión debe responder una pregunta antes de comenzar a programar en serio: **¿por qué existe Quarkus?**

La sesión presenta el cambio de contexto que motivó su diseño, introduce la idea de build time frente a runtime, ubica Quarkus dentro del ecosistema Java y explica a alto nivel el papel de las extensiones. La parte práctica termina con dos proyectos mínimos: la demo aislada y el Issue Tracker.

Los comandos completos utilizados durante la grabación están en `commands.md`. En este guion solo se indican las acciones que conviene realizar y lo que interesa destacar mientras se muestran.

---

## 1. Apertura: ¿por qué otro framework Java?

### En pantalla

Portada:

**Quarkus a Fondo**  
**01 — Qué es Quarkus y qué problema intenta resolver**

Después, dejar solamente:

> ¿Por qué otro framework Java?

### Narración

Java ya tiene un ecosistema enorme. Jakarta EE, Hibernate, MicroProfile y muchas bibliotecas maduras llevan años resolviendo aplicaciones empresariales.

Por eso, empezar diciendo que Quarkus es rápido, que consume poca memoria o que puede generar ejecutables nativos no responde todavía la pregunta importante.

Antes de ver cómo se crea una aplicación Quarkus conviene entender qué cambió para que apareciera otra propuesta dentro de un ecosistema tan maduro.

---

## 2. El contexto cambió

### En pantalla

Mostrar una aplicación empresarial que arranca una vez y permanece ejecutándose durante horas, días o meses. Después introducir visualmente cloud, contenedores, escalamiento dinámico, alta densidad y scale-to-zero.

### Narración

En muchos sistemas empresariales tradicionales una aplicación arrancaba una vez y después permanecía funcionando durante mucho tiempo.

En ese escenario, realizar bastante trabajo durante el arranque podía ser perfectamente razonable. El framework podía descubrir componentes, procesar metadatos, preparar estructuras internas y amortizar ese coste durante una ejecución prolongada.

Ese modelo sigue siendo válido. Lo que cambió es que hoy también desplegamos aplicaciones en escenarios donde las instancias pueden aparecer y desaparecer, escalar según la carga o compartir infraestructura con muchas otras aplicaciones.

Ahí adquieren especial importancia dos características: cuánto tarda una aplicación en estar lista y cuántos recursos necesita.

El punto no es presentar los modelos tradicionales como un error, sino mostrar que aparecieron condiciones distintas y que Quarkus fue diseñado prestando atención a ellas.

---

## 3. La idea central: build time y runtime

### En pantalla

Plantear la pregunta:

> Si una parte de la aplicación ya puede conocerse durante el build, ¿tenemos que descubrirla otra vez cada vez que arrancamos?

Mostrar después los dos diagramas simplificados de build y runtime incluidos en las diapositivas.

### Narración

Aquí aparece una de las ideas fundamentales de Quarkus.

Si durante la construcción del artefacto ya tenemos información suficiente para realizar parte de la preparación del framework, podemos intentar mover ese trabajo hacia el build.

En un modelo tradicional simplificado, una cantidad importante de preparación puede ocurrir cuando la aplicación arranca. Quarkus intenta realizar antes todo aquello que razonablemente pueda resolverse durante la construcción.

Eso puede reducir el trabajo necesario al arrancar y contribuir a mejorar tiempo de inicio y uso de recursos.

Conviene remarcar que el diagrama es conceptual. Quarkus no hace absolutamente todo durante el build y el runtime sigue teniendo trabajo que realizar.

Tampoco es momento de explicar augmentation, build steps o class loading. Esos mecanismos pertenecen a una ruta posterior. En esta sesión basta con entender la dirección arquitectónica.

---

## 4. Quarkus dentro del ecosistema Java

### En pantalla

Mostrar Quarkus rodeado por Jakarta EE, MicroProfile, Hibernate ORM, SmallRye y Vert.x.

### Narración

Quarkus se apoya en tecnologías conocidas del ecosistema Java.

Durante el curso utilizaremos estándares, APIs e implementaciones que ya existen, y aprenderemos progresivamente qué parte corresponde a cada proyecto y qué aporta Quarkus en su integración.

Esta distinción será importante más adelante. Por ahora basta con ubicar Quarkus como un stack que integra ese ecosistema dentro de un modelo de construcción y ejecución diseñado para aplicaciones modernas.

---

## 5. Extensiones

### En pantalla

Mostrar una aplicación Quarkus a la que se van incorporando capacidades como REST, Hibernate ORM, PostgreSQL, OIDC y OpenTelemetry.

### Narración

Las capacidades de una aplicación Quarkus se incorporan en gran medida mediante extensiones.

Esto encaja con la metodología del curso: no instalaremos desde el principio todo lo que creemos que necesitaremos. Cada capacidad aparecerá cuando tengamos un problema que justifique aprenderla.

Una extensión involucra dependencias Maven, pero su integración puede abarcar también el build, la configuración, el desarrollo y el runtime.

No necesitamos estudiar todavía esos mecanismos. La siguiente sesión abrirá precisamente la caja de Maven, el plugin de Quarkus, las extensiones y la estructura generada.

---

## 6. Malentendidos frecuentes

### En pantalla

Mostrar de forma breve cuatro asociaciones frecuentes:

- Quarkus y Native Image;
- Quarkus y Kubernetes;
- Quarkus y Jakarta EE;
- Quarkus y programación reactiva.

### Narración

Quarkus puede ejecutarse perfectamente sobre la JVM. Native Image será una alternativa de empaquetado que estudiaremos más adelante.

Aunque su diseño presta especial atención a cloud y contenedores, sus decisiones también pueden ser útiles fuera de Kubernetes.

El framework utiliza tecnologías del ecosistema Java en lugar de reemplazarlas.

Y tampoco necesitamos empezar escribiendo código reactivo. Cuando lleguemos a modelos de ejecución podremos estudiar blocking, non-blocking, Mutiny y virtual threads con un problema concreto delante.

---

## 7. Preparar el entorno

### Transición

Hasta aquí todo ha sido conceptual. Ahora vamos a comprobar que tenemos el entorno mínimo para crear la primera aplicación.

### En pantalla

Mostrar el baseline:

**JDK 25 · Maven 3.9.16 · Git**

Si la máquina todavía no tiene Java o Maven preparados, abrir los Gists del curso, descargar los scripts y ejecutarlos. No explicar su implementación; solo mostrar que permiten preparar el entorno de forma reproducible.

Para Maven, enfatizar que la versión siempre se pasa explícitamente al script y no se depende de su valor por defecto.

Después mostrar las verificaciones de Java, Maven y Git definidas en `commands.md`.

### Narración

El curso necesita tres herramientas base: JDK 25, Maven 3.9.16 y Git.

Los scripts publicados para Java y Maven son utilidades de preparación del entorno. No forman parte del contenido técnico de Quarkus y no vamos a analizarlos línea por línea.

Lo importante aquí es que Maven esté utilizando el JDK esperado y que las versiones queden claras antes de generar el proyecto.

La Quarkus CLI no es un requisito del curso. Todo el recorrido principal se hará con Maven.

---

## 8. Demo aislada: primera aplicación

### En pantalla

Cambiar al terminal y situarse en `demos/`.

Ejecutar el comando de creación documentado en `commands.md` para generar `demos/01-getting-started/`.

### Narración

Nuestra primera demo vive en `demos/01-getting-started/`.

Para crearla invocamos directamente el plugin Maven de Quarkus. En esta sesión no vamos a explicar todos los parámetros del comando; eso pertenece a la siguiente.

### En pantalla

Abrir brevemente el proyecto y señalar, sin desarrollarlos, el `pom.xml`, `src/main/java/`, `src/main/resources/` y `src/test/`.

### Narración

El proyecto ya contiene estructura Maven, código Java, configuración y pruebas generadas.

Es tentador empezar a recorrer cada archivo, pero todavía no tenemos el contexto para hacerlo con provecho. Hoy solo queremos comprobar que la aplicación funciona.

### En pantalla

Ejecutar la aplicación usando el comando de `commands.md` y, desde otra terminal, realizar la verificación HTTP.

No demostrar todavía live coding ni recorrer Dev UI.

### Narración

Con esto la demo ha cumplido su propósito.

No estamos midiendo rendimiento, no estamos compilando Native Image y tampoco estamos estudiando las anotaciones del recurso generado.

Solo hemos creado una aplicación Quarkus con Maven, la hemos arrancado y hemos comprobado que responde.

---

## 9. Lo que queda para después

### En pantalla

Mostrar una correspondencia breve:

```text
pom.xml / plugin / extensiones / estructura  → sesión 02
Dev Mode                                     → sesión 03
Dev UI                                       → sesión 04
configuración                                → sesión 05
CDI / ArC                                    → sesión 07
```

### Narración

Estamos utilizando algunas herramientas antes de estudiarlas a fondo, y eso es intencional.

La prioridad hoy es entender para qué existe Quarkus y terminar con una aplicación funcionando. Las piezas que acabamos de tocar superficialmente tendrán sesiones propias.

Incluso las pruebas generadas se quedan sin explicar. Cuando llegue el bloque de testing veremos cómo probar una aplicación Quarkus de forma sistemática.

---

## 10. Del laboratorio al Issue Tracker

### Transición

Cada sesión del curso tendrá dos movimientos: primero una demo aislada y después la aplicación transversal.

### En pantalla

Mostrar la estructura:

```text
quarkus-a-fondo/
├── docs/
├── demos/
│   └── 01-getting-started/
└── issue-tracker/
```

Crear `issue-tracker/` con el comando correspondiente de `commands.md`, ejecutarlo y comprobar que responde.

### Narración

En esta primera sesión el Issue Tracker no necesita funcionalidad de negocio.

Todavía no tendremos Projects, Issues, usuarios, persistencia ni capas. La primera evolución consiste únicamente en crear el proyecto base y comprobar que puede ejecutarse.

La demo y el Issue Tracker pueden parecer casi idénticos ahora, pero cumplen funciones distintas.

La demo queda como laboratorio aislado de esta sesión. El Issue Tracker conserva su historia y evolucionará durante todo el curso.

---

## 11. Cierre

### En pantalla

Recuperar las cuatro ideas centrales:

- cambio de contexto;
- build time y runtime;
- ecosistema Java;
- extensiones.

### Narración

De esta primera sesión conviene conservar cuatro ideas.

El contexto de ejecución de las aplicaciones cambió y aparecen escenarios donde startup y consumo de recursos tienen especial importancia.

Quarkus intenta trasladar hacia build time parte del trabajo que puede resolverse antes de arrancar.

Se apoya en tecnologías del ecosistema Java que iremos estudiando durante el curso.

Y una aplicación Quarkus adquiere capacidades mediante extensiones.

Ya tenemos además dos proyectos: una demo congelada como referencia de esta sesión y un Issue Tracker que recién empieza su recorrido.

La siguiente sesión abrirá la caja que hoy utilizamos casi sin mirar: Maven, el plugin de Quarkus, el `pom.xml`, las extensiones y la estructura del proyecto.

## Notas para la grabación

- El curso utiliza JDK 25, Maven 3.9.16 y Git como baseline actual.
- Maven es la herramienta principal; no usar Quarkus CLI.
- Los scripts de instalación se muestran solo como utilidades de preparación. No explicar su implementación.
- Al ejecutar `setup-mvn.ps1`, pasar siempre `-MavenVersion 3.9.16`.
- Los comandos completos pertenecen a `commands.md`.
- No medir startup ni memoria como benchmark.
- No compilar Native Image.
- No demostrar live coding todavía.
- No explicar las anotaciones del endpoint generado.
- No explicar los tests generados.
- No recorrer exhaustivamente el `pom.xml`.
