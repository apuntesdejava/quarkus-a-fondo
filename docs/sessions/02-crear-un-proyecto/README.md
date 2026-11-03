# Sesión 02 — Crear un proyecto: Maven, extensiones y estructura

## Objetivo de aprendizaje

Al terminar la sesión, el estudiante debería poder crear un proyecto Quarkus con Maven, reconocer su estructura básica, entender el papel del `pom.xml`, del BOM y del plugin de Quarkus, identificar las extensiones instaladas y agregar o eliminar extensiones sin tratar el proyecto generado como una caja negra.

La sesión parte de lo construido anteriormente. En la sesión 01 creamos y ejecutamos una aplicación Quarkus; ahora toca entender qué se generó y cómo se administra.

## Video

[Ver la sesión 02 en YouTube](https://youtu.be/Q01st1rf9Kk)

## Entorno base

La sesión utiliza el baseline actual del curso:

- JDK 25;
- Apache Maven 3.9.16;
- Git;
- Quarkus 3.40.1.1.

Maven continúa siendo la herramienta principal. La Quarkus CLI no forma parte de los requisitos del curso.

## Crear un proyecto con Maven

Desde la carpeta `demos/` se crea una aplicación dedicada a esta sesión:

```powershell
mvn "io.quarkus.platform:quarkus-maven-plugin:3.40.1.1:create" `
    "-DprojectGroupId=io.github.apuntesdejava.quarkusafondo" `
    "-DprojectArtifactId=02-project-structure" `
    "-Dextensions=rest"
```

El comando invoca el plugin Maven de Quarkus para crear un proyecto con las coordenadas indicadas e incorporar la extensión REST.

En la sesión anterior utilizamos un comando equivalente para obtener una aplicación ejecutable. En esta ocasión nos interesa observar el resultado con más detalle.

## Estructura generada

El proyecto generado contiene, entre otros elementos:

```text
02-project-structure/
├── .mvn/
│   └── wrapper/
│       └── maven-wrapper.properties
├── src/
│   ├── main/
│   │   ├── java/
│   │   ├── resources/
│   │   └── docker/
│   └── test/
├── mvnw
├── mvnw.cmd
├── pom.xml
└── README.md
```

No todas estas piezas necesitan estudiarse al mismo tiempo.

En esta sesión nos interesan principalmente:

- `pom.xml`;
- Maven Wrapper;
- `src/main/java`;
- `src/main/resources`;
- el `README.md` generado.

Otros elementos se conservarán, pero se explicarán cuando corresponda:

- `src/test/` se estudiará en el bloque de testing;
- `src/main/docker/` se retomará al estudiar contenedores;
- `application.properties` se utilizará en las sesiones de configuración;
- las anotaciones del recurso REST se explicarán al estudiar Jakarta REST y Quarkus REST.

La presencia de una capacidad en el proyecto generado no obliga a estudiarla inmediatamente.

## Maven Wrapper

Quarkus genera también Maven Wrapper. En Windows puede ejecutarse con:

```powershell
.\mvnw.cmd -version
```

El archivo:

```text
.mvn/wrapper/maven-wrapper.properties
```

indica qué distribución de Maven utilizará el wrapper. En la demo creada para esta sesión contiene Maven 3.9.16:

```properties
wrapperVersion=3.3.4
distributionType=only-script
distributionUrl=https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.9.16/apache-maven-3.9.16-bin.zip
distributionSha256Sum=5af3b743dd8b876b5c45da33b676251e5f1687712644abb4ee519ca56e1d89ce
```

Esto permite que el proyecto indique una versión concreta de Maven sin depender exclusivamente de la instalación global del equipo.

No necesitamos estudiar la implementación interna de `mvnw` o `mvnw.cmd`; basta con entender para qué existen.

## El `pom.xml`

El `pom.xml` concentra buena parte de la integración entre Maven y Quarkus.

### Identidad del proyecto

El proyecto conserva las coordenadas Maven habituales:

```xml
<groupId>io.github.apuntesdejava.quarkusafondo</groupId>
<artifactId>02-project-structure</artifactId>
<version>1.0.0-SNAPSHOT</version>
<packaging>quarkus</packaging>
```

Estas coordenadas identifican el proyecto y su artefacto.

### Plataforma Quarkus

El proyecto generado define la plataforma de Quarkus mediante propiedades:

```xml
<quarkus.platform.artifact-id>quarkus-bom</quarkus.platform.artifact-id>
<quarkus.platform.group-id>io.quarkus.platform</quarkus.platform.group-id>
<quarkus.platform.version>3.40.1.1</quarkus.platform.version>
```

La versión de la plataforma es relevante porque establece el conjunto de componentes Quarkus que se utilizarán de forma coordinada.

### BOM y gestión de versiones

El proyecto importa el BOM de Quarkus mediante `dependencyManagement`:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>${quarkus.platform.group-id}</groupId>
            <artifactId>${quarkus.platform.artifact-id}</artifactId>
            <version>${quarkus.platform.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

El BOM administra las versiones de las dependencias que forman parte de la plataforma. Por eso una dependencia como:

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-rest</artifactId>
</dependency>
```

no necesita declarar una versión propia.

La relación puede resumirse así:

```text
Quarkus Platform
      │
      ▼
     BOM
      │
      ▼
versiones compatibles
      │
      ▼
dependencias del proyecto
```

Esto evita que cada extensión de la plataforma tenga que seleccionar su versión de manera independiente.

## Quarkus Maven Plugin

El proyecto incluye también el plugin Maven de Quarkus:

```xml
<plugin>
    <groupId>${quarkus.platform.group-id}</groupId>
    <artifactId>quarkus-maven-plugin</artifactId>
    <version>${quarkus.platform.version}</version>
    <extensions>true</extensions>
</plugin>
```

El plugin proporciona los goals utilizados para crear y administrar el proyecto, además de otras capacidades que iremos utilizando a lo largo del curso.

En esta sesión nos interesan especialmente los goals relacionados con extensiones.

## Extensiones

Una aplicación Quarkus incorpora capacidades mediante extensiones.

El proyecto se creó inicialmente con REST. Podemos comprobar las extensiones instaladas con:

```powershell
mvn quarkus:list-extensions "-Dinstalled=true"
```

En la demo inicial el resultado relevante es:

```text
quarkus-rest    REST
```

### Agregar una extensión

Para observar cómo cambia el proyecto, agregamos temporalmente SmallRye Health:

```powershell
mvn quarkus:add-extension "-Dextensions=smallrye-health"
```

El cambio relevante en el `pom.xml` es:

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-smallrye-health</artifactId>
</dependency>
```

La dependencia tampoco declara una versión porque forma parte de la gestión proporcionada por la plataforma.

Al volver a listar las extensiones instaladas:

```powershell
mvn quarkus:list-extensions "-Dinstalled=true"
```

aparecen ambas:

```text
quarkus-rest
quarkus-smallrye-health
```

En esta sesión SmallRye Health se utiliza únicamente para observar la administración de extensiones. Su funcionamiento se estudiará más adelante.

### Eliminar una extensión

La extensión temporal se elimina con:

```powershell
mvn quarkus:remove-extension "-Dextensions=smallrye-health"
```

La demo queda nuevamente con REST como única extensión seleccionada.

Este experimento permite ver una relación importante:

```text
extensión Quarkus
       │
       ▼
dependencia Maven
       │
       ▼
versión administrada por la plataforma
```

Una extensión Quarkus no debe entenderse únicamente como una dependencia Maven, pero desde el proyecto podemos observar cómo su incorporación queda representada en el `pom.xml`.

Los detalles internos de una extensión y su participación en el proceso de construcción pertenecen a temas avanzados del curso.

## Código Java generado

La extensión REST utilizada al crear el proyecto genera un recurso de ejemplo en:

```text
src/main/java/io/github/apuntesdejava/quarkusafondo/GreetingResource.java
```

El archivo pertenece al paquete:

```java
package io.github.apuntesdejava.quarkusafondo;
```

Esto permite reconocer la relación entre la estructura de directorios y los paquetes Java.

El recurso utiliza anotaciones de Jakarta REST como `@Path`, `@GET` y `@Produces`. En esta sesión no necesitamos estudiarlas todavía; se explicarán cuando lleguemos al bloque dedicado a APIs REST.

## Recursos y configuración

El proyecto genera:

```text
src/main/resources/application.properties
```

En esta demo el archivo está inicialmente vacío.

Su presencia indica dónde podremos configurar la aplicación, pero el modelo de configuración de Quarkus, SmallRye Config, perfiles y fuentes de configuración se estudiará en sesiones posteriores.

## Pruebas generadas

Quarkus crea también pruebas para el recurso de ejemplo:

```text
GreetingResourceTest.java
GreetingResourceIT.java
```

Estos archivos utilizan capacidades como `@QuarkusTest`, `@QuarkusIntegrationTest` y REST Assured.

Se conservan en el proyecto, pero no se explican todavía. El testing tendrá su propio bloque y, a partir de allí, acompañará de forma habitual al resto del curso.

## Dockerfiles generados

El proyecto contiene varios Dockerfiles en:

```text
src/main/docker/
```

Por ejemplo:

```text
Dockerfile.jvm
Dockerfile.legacy-jar
Dockerfile.native
Dockerfile.native-micro
```

Estos archivos están preparados para distintos escenarios de empaquetado y ejecución.

No forman parte del objetivo de esta sesión. Los estudiaremos cuando lleguemos al bloque de empaquetado, contenedores y Native Image.

## `README.md` generado

Quarkus genera también un `README.md` con instrucciones para ejecutar, empaquetar y continuar trabajando con la aplicación.

Ese archivo resulta útil como referencia inmediata del proyecto, pero incluye temas como Dev Mode, Dev UI, empaquetado y Native Image que todavía no necesitamos desarrollar.

La sesión 02 se concentra únicamente en las partes necesarias para entender la estructura básica y administrar extensiones.

## Demo aislada: `demos/02-project-structure/`

La demo sigue este recorrido:

```text
crear proyecto
      │
      ▼
observar estructura
      │
      ▼
examinar pom.xml
      │
      ▼
entender plataforma y BOM
      │
      ▼
revisar Maven Wrapper
      │
      ▼
listar extensiones instaladas
      │
      ▼
agregar SmallRye Health
      │
      ▼
observar el cambio en pom.xml
      │
      ▼
eliminar SmallRye Health
      │
      ▼
volver al estado base
```

El objetivo no es aprender REST ni Health. Ambas extensiones sirven aquí para entender cómo se construye y administra un proyecto Quarkus.

## Aplicación transversal: Issue Tracker

El Issue Tracker fue creado en la sesión anterior y permanece en:

```text
issue-tracker/
```

No necesita recrearse ni recibir nueva funcionalidad en esta sesión.

Su `pom.xml` utiliza la misma estructura básica que acabamos de estudiar:

- plataforma Quarkus;
- BOM;
- dependencias;
- `quarkus-maven-plugin`;
- Maven Wrapper;
- estructura estándar de código y recursos.

Podemos verificar sus extensiones instaladas con:

```powershell
mvn quarkus:list-extensions "-Dinstalled=true"
```

El resultado esperado en esta etapa es:

```text
quarkus-rest    REST
```

No agregamos extensiones ni creamos paquetes o capas adicionales en el Issue Tracker. Su estructura seguirá creciendo únicamente cuando aparezcan responsabilidades concretas que justifiquen hacerlo.

## Resultado final de la sesión

Al finalizar:

- existe `demos/02-project-structure/`;
- la demo vuelve a quedar únicamente con REST después del experimento con SmallRye Health;
- el estudiante reconoce las partes principales de un proyecto Quarkus;
- entiende qué papel cumplen la plataforma, el BOM y el plugin Maven de Quarkus;
- puede listar, agregar y eliminar extensiones;
- entiende para qué sirve Maven Wrapper;
- identifica elementos generados que se estudiarán en sesiones posteriores;
- el Issue Tracker conserva su estado funcional de la sesión 01, pero ahora conocemos mejor cómo está construido.

## Conexión con la sesión 03

Ya sabemos qué contiene el proyecto y cómo administrar sus extensiones.

La siguiente sesión se concentra en una de las capacidades más importantes de la experiencia de desarrollo de Quarkus:

**Sesión 03 — Dev Mode y ciclo de desarrollo.**

## Fuentes oficiales

Fuentes principales utilizadas para preparar y verificar esta sesión:

- Quarkus — Creating your first application: https://quarkus.io/guides/getting-started/
- Quarkus — Quarkus and Maven: https://quarkus.io/guides/maven-tooling
- Quarkus — Quarkus Maven plugin: https://quarkus.io/version/3.40/guides/quarkus-maven-plugin/
