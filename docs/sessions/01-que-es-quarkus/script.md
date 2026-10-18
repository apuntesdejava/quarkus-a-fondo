# Guion — Sesión 01: Qué es Quarkus y qué problema intenta resolver

## Propósito de la sesión

Esta primera sesión debe responder una pregunta antes de comenzar a programar en serio:

**¿por qué existe Quarkus?**

El objetivo no es recorrer características ni aprender todavía la estructura completa de una aplicación. La sesión presenta el problema, introduce la idea de build time frente a runtime, ubica Quarkus dentro del ecosistema Java, explica a alto nivel el papel de las extensiones y termina creando y ejecutando dos proyectos mínimos: la demo aislada y el Issue Tracker.

El flujo principal del curso utiliza **Java + Maven**. No se instalará ni utilizará Quarkus CLI. Esto reduce herramientas adicionales y deja visible desde el principio que estamos trabajando con proyectos Java/Maven y con el plugin Maven de Quarkus.

---

## 1. Apertura: ¿por qué otro framework Java?

### En pantalla

Portada:

**Quarkus a Fondo**  
**01 — Qué es Quarkus y qué problema intenta resolver**

Después, dejar solamente:

> ¿Por qué otro framework Java?

### Narración

Java ya tiene un ecosistema enorme. Tenemos Jakarta EE, Hibernate, MicroProfile, bibliotecas maduras y frameworks que llevan años funcionando en producción.

Por eso, empezar diciendo que Quarkus es rápido, que consume poca memoria o que puede generar ejecutables nativos no responde todavía la pregunta importante.

Antes de ver cómo se crea una aplicación Quarkus quiero entender por qué apareció otra propuesta dentro de un ecosistema que ya resolvía aplicaciones empresariales desde hace mucho tiempo.

La clave está en que el contexto de ejecución cambió.

---

## 2. El contexto cambió

### En pantalla

Mostrar primero:

```text
Aplicación empresarial tradicional

arranca
   │
   ▼
permanece ejecutándose
   │
   ▼
horas / días / meses
```

Después introducir:

```text
cloud
contenedores
escalamiento dinámico
alta densidad
scale-to-zero
```

### Narración

En muchos sistemas empresariales tradicionales una aplicación arrancaba una vez y después permanecía funcionando durante días o meses.

En ese escenario, realizar bastante trabajo durante el arranque podía ser perfectamente razonable. El framework podía descubrir componentes, procesar metadatos, preparar estructuras internas y después amortizar ese coste durante una ejecución prolongada.

Ese modelo sigue siendo válido.

Lo que cambió es que hoy también desplegamos aplicaciones en escenarios donde las instancias pueden aparecer y desaparecer, escalar según la carga o compartir infraestructura con muchas otras aplicaciones.

Ahí adquieren especial importancia dos cosas: **cuánto tarda una aplicación en estar lista y cuántos recursos necesita**.

No estamos diciendo que Java tradicional esté mal. Estamos diciendo que surgieron condiciones diferentes y que Quarkus fue diseñado prestando atención a ellas.

---

## 3. La idea central: hacer antes lo que podamos hacer antes

### En pantalla

Pregunta:

> Si una parte de la aplicación ya puede conocerse durante el build, ¿tenemos que descubrirla otra vez cada vez que arrancamos?

Después mostrar:

```text
build time  ───────────────►  runtime
```

### Narración

Aquí aparece una de las ideas fundamentales de Quarkus.

Si durante la construcción del artefacto ya tenemos información suficiente para realizar parte de la preparación del framework, podemos intentar mover ese trabajo hacia el build.

Veámoslo de forma deliberadamente simplificada.

### En pantalla

```text
Enfoque tradicional

build
  │
  ▼
artefacto
  │
  ▼
arranque
  │
  ├── preparación
  ├── descubrimiento
  ├── metadatos
  └── inicialización
  │
  ▼
aplicación lista
```

### Narración

Podemos imaginar un modelo donde una cantidad importante de preparación ocurre cuando la aplicación arranca.

Ahora veamos la dirección que toma Quarkus.

### En pantalla

```text
Quarkus

build
  │
  ├── análisis
  └── preparación
  │
  ▼
artefacto preparado
  │
  ▼
arranque
  │
  ▼
aplicación lista
```

### Narración

Quarkus intenta realizar durante el build todo el trabajo que razonablemente pueda hacerse allí.

Eso puede reducir el trabajo necesario al arrancar y contribuir a mejorar el tiempo de inicio y el uso de recursos.

El diagrama es conceptual. No significa que Quarkus haga absolutamente todo durante el build ni que el runtime deje de realizar inicialización.

Tampoco vamos a explicar todavía cómo funciona internamente. Augmentation, build steps, class loading y otros detalles tendrán su lugar más adelante.

En esta sesión basta con entender la dirección arquitectónica.

---

## 4. Quarkus dentro del ecosistema Java

### En pantalla

```text
              Quarkus
                 │
      ┌──────────┼──────────┐
      │          │          │
  Jakarta EE  MicroProfile  Hibernate
      │          │          │
      └──── ecosistema Java ┘
```

Agregar visualmente nombres como SmallRye y Vert.x sin convertir la diapositiva en un catálogo.

### Narración

Quarkus no pretende reemplazar el ecosistema Java.

Durante el curso vamos a encontrarnos con Jakarta EE, MicroProfile, Hibernate ORM, SmallRye, Vert.x y otras tecnologías existentes.

Quarkus las integra dentro de su modelo de construcción, configuración, desarrollo y ejecución.

Conforme avancemos habrá que distinguir entre la API o estándar que utilizamos, su implementación y la integración que Quarkus realiza.

Hoy solamente necesitamos quedarnos con una idea: aprender Quarkus no significa abandonar el ecosistema Java. Significa utilizarlo dentro de un runtime y una experiencia de desarrollo diseñados con otras prioridades.

---

## 5. Extensiones

### En pantalla

Construir progresivamente:

```text
Aplicación Quarkus
      │
      ├── REST
      ├── Hibernate ORM
      ├── PostgreSQL
      ├── OIDC
      └── OpenTelemetry
```

### Narración

Una aplicación Quarkus incorpora muchas de sus capacidades mediante extensiones.

Si necesitamos exponer una API, tendremos una extensión para REST. Cuando lleguemos a persistencia aparecerán Hibernate ORM y PostgreSQL. Cuando lleguemos a seguridad aparecerá OIDC.

Esto coincide con la metodología del curso: no vamos a instalar desde el principio todo lo que creemos que necesitaremos. Cada capacidad aparecerá cuando exista un problema que justifique aprenderla.

Hay otro detalle que conviene dejar claro desde ahora.

### En pantalla

```text
Extensión Quarkus
       ≠
"solo otra dependencia Maven"
```

### Narración

Una extensión termina involucrando dependencias Maven, pero no debemos reducirla mentalmente a eso. Puede integrarse con el build, configuración, desarrollo y runtime de Quarkus.

Todavía no vamos a estudiar esos mecanismos.

La siguiente sesión será justamente donde veremos cómo Maven y el plugin de Quarkus crean el proyecto y cómo se incorporan extensiones.

---

## 6. Cuatro malentendidos que conviene evitar

### En pantalla

Mostrar uno por uno:

**Quarkus = Native Image** ❌  
**Quarkus = solo Kubernetes** ❌  
**Quarkus reemplaza Jakarta EE** ❌  
**Quarkus obliga a programar reactivo** ❌

### Narración

Antes de pasar a la práctica quiero evitar cuatro asociaciones que pueden confundir el resto del curso.

Quarkus no significa Native Image. Podemos ejecutar perfectamente nuestra aplicación sobre la JVM. Native será una alternativa de empaquetado que estudiaremos mucho más adelante.

Quarkus tampoco sirve únicamente para Kubernetes. Su diseño presta mucha atención a cloud y contenedores, pero sus decisiones también pueden ser útiles fuera de esos entornos.

No reemplaza Jakarta EE ni el resto del ecosistema Java; acabamos de ver que se apoya precisamente en muchas de esas tecnologías.

Y utilizar Quarkus no significa que debamos programar todo de forma reactiva. Cuando lleguemos a modelos de ejecución estudiaremos blocking, non-blocking, Mutiny y virtual threads con un problema concreto delante.

---

## 7. Preparar el entorno de la demo

### Transición

Hasta aquí todo ha sido conceptual. Ahora vamos a crear la primera aplicación.

Para seguir el curso solamente necesitamos tener Java y Maven preparados. No necesitamos instalar la CLI de Quarkus.

### En pantalla

Comprobar:

```powershell
java -version
mvn -version
```

### Narración

Antes de crear el proyecto verificamos dos cosas: qué Java está utilizando la terminal y qué Maven está utilizando ese Java.

Si estamos preparando Maven desde cero con el script del curso, **siempre indicaremos explícitamente la versión**.

### En pantalla

```powershell
.\setup-mvn.ps1 -MavenVersion 3.9.16
```

### Narración

No dejamos que el curso dependa del valor por defecto que pueda tener el script. La versión forma parte del entorno reproducible del curso.

En este momento la guía oficial de Getting Started de Quarkus utiliza Maven 3.9.16 como prerrequisito. Si actualizamos la versión utilizada por el curso en el futuro, actualizaremos también este comando y la documentación correspondiente.

Después de instalar o configurar Maven comprobamos nuevamente:

```powershell
mvn -version
```

Además de la versión de Maven, este comando nos permite verificar qué JDK está utilizando Maven.

---

## 8. Demo aislada: crear la primera aplicación con Maven

### En pantalla

Cambiar al terminal en la carpeta:

```text
quarkus-a-fondo/
└── demos/
```

### Narración

Nuestra primera demo será `demos/01-getting-started`.

Para crearla no utilizaremos una herramienta adicional. Invocaremos directamente el plugin Maven de Quarkus.

No voy a explicar todavía cada parámetro del comando. La siguiente sesión estará dedicada precisamente a Maven, extensiones y estructura del proyecto.

### En pantalla

En PowerShell:

```powershell
mvn io.quarkus.platform:quarkus-maven-plugin:3.40.1:create `
    "-DprojectGroupId=io.github.apuntesdejava.quarkusafondo" `
    "-DprojectArtifactId=01-getting-started" `
    "-Dextensions=rest"
```

### Narración

Por ahora solo observemos el resultado.

### En pantalla

Abrir el proyecto y señalar brevemente:

```text
pom.xml
src/main/java/
src/main/resources/
src/test/
```

### Narración

Tenemos un proyecto Maven, código Java, configuración y pruebas generadas.

Es tentador empezar a abrir cada archivo, pero no lo haremos todavía.

Hoy queremos ejecutar la aplicación.

### En pantalla

Desde `demos/01-getting-started`:

```powershell
mvn quarkus:dev
```

Mostrar el arranque.

### Narración

El objetivo de esta sesión no es estudiar Dev Mode. Por ahora solamente estamos utilizando el goal que nos permite ejecutar la aplicación durante el desarrollo.

Dev Mode tendrá su propia sesión.

### En pantalla

Realizar:

```text
GET http://localhost:8080/hello
```

Mostrar la respuesta.

### Narración

Con esto la primera demo ha cumplido su propósito.

No hicimos un benchmark. No compilamos una aplicación nativa. No explicamos todavía las anotaciones del recurso generado.

Simplemente comprobamos que podemos crear una aplicación Quarkus con Maven, arrancarla y obtener una respuesta.

---

## 9. Lo que estamos dejando sin explicar

### En pantalla

```text
pom.xml             → sesión 02
plugin Maven        → sesión 02
extensiones         → sesión 02
estructura          → sesión 02
mvn quarkus:dev     → sesión 03
Dev UI              → sesión 04
configuración       → sesión 05
CDI / ArC           → sesión 07
```

### Narración

Estamos utilizando algunas herramientas antes de estudiarlas a fondo, y eso es intencional.

Prefiero que hoy sepamos para qué existe Quarkus y terminemos con algo funcionando, en lugar de comprimir varias sesiones en una sola introducción.

Incluso la prueba que Quarkus haya generado se queda sin explicar. Cuando lleguemos al bloque de testing estudiaremos cómo probar correctamente la aplicación.

---

## 10. Del laboratorio al Issue Tracker

### Transición

La metodología del curso tendrá siempre dos movimientos: primero una demo aislada y después la aplicación transversal.

Nuestra aplicación transversal será un Issue Tracker.

### En pantalla

```text
quarkus-a-fondo/
├── docs/
├── demos/
│   └── 01-getting-started/
└── issue-tracker/
```

### Narración

En esta primera sesión el Issue Tracker no necesita ninguna funcionalidad de negocio.

No tendremos Projects, Issues, usuarios, persistencia ni capas.

La primera evolución consiste solamente en crear el proyecto base y comprobar que puede ejecutarse.

### En pantalla

Crear `issue-tracker` también mediante el plugin Maven de Quarkus, utilizando la misma versión de plataforma elegida para la sesión.

Después ejecutar:

```powershell
mvn quarkus:dev
```

y comprobar que arranca.

### Narración

La demo y el Issue Tracker pueden parecer casi idénticos en este momento. La diferencia está en su propósito.

### En pantalla

```text
demos/01-getting-started
        │
        ▼
experimento de esta sesión
```

```text
issue-tracker
        │
        ▼
aplicación que evoluciona
durante todo el curso
```

### Narración

La demo queda como un laboratorio pequeño y local al concepto.

El Issue Tracker conserva su historia. Cada sesión irá agregándole solamente aquello que ya hayamos aprendido.

---

## 11. Cierre

### En pantalla

Mostrar progresivamente:

```text
El contexto cambió
        │
        ▼
más trabajo en build time
        │
        ▼
menos trabajo al arrancar
```

Y debajo:

**ecosistema Java + extensiones**

### Narración

De esta primera sesión quiero que queden cuatro ideas.

El contexto de ejecución de las aplicaciones cambió y aparecen escenarios donde startup y recursos importan especialmente.

Quarkus intenta trasladar hacia build time parte del trabajo que tradicionalmente puede ocurrir durante el arranque.

No reemplaza el ecosistema Java: integra tecnologías que iremos encontrando durante el curso.

Y las capacidades de una aplicación Quarkus se incorporan mediante extensiones.

Ya tenemos además nuestras dos primeras aplicaciones: una demo que puede quedarse congelada como referencia de esta sesión y un Issue Tracker que recién empieza su recorrido.

### En pantalla

Resaltar:

```text
Maven
pom.xml
plugin de Quarkus
extensiones
estructura
```

### Narración

Pero todavía estamos usando una caja que no hemos abierto.

¿Qué hizo exactamente ese comando Maven?

¿Qué papel tiene el plugin de Quarkus?

¿Qué contiene el `pom.xml`?

¿Cómo agregamos o quitamos extensiones?

Y ¿qué partes de la estructura generada necesitamos conocer realmente?

Eso será la siguiente sesión:

**Crear un proyecto: Maven, extensiones y estructura.**

---

## Notas para la grabación

- El curso utiliza **Java y Maven**, no Quarkus CLI.
- Si se muestra `setup-mvn.ps1`, pasar siempre `-MavenVersion`; nunca depender del valor por defecto del script.
- Para esta versión de la sesión se utiliza Maven **3.9.16**, coherente con el prerrequisito indicado por la guía oficial actual de Getting Started de Quarkus.
- Antes de grabar, verificar que la versión del plugin Maven de Quarkus mostrada en el comando sigue siendo la versión elegida para el curso.
- Utilizar PowerShell en los comandos de Windows y envolver los parámetros `-D...` entre comillas.
- No medir startup ni memoria como benchmark.
- No compilar Native Image.
- No demostrar live coding todavía.
- No explicar las anotaciones del endpoint generado.
- No explicar el test generado.
- No recorrer exhaustivamente el `pom.xml`.
- La demo debe reducir abstracción, no adelantar contenido de sesiones futuras.

## Referencias de preparación

- https://quarkus.io/get-started/
- https://quarkus.io/guides/getting-started
- https://quarkus.io/guides/maven-tooling
- https://quarkus.io/about/
- https://quarkus.io/container-first/
