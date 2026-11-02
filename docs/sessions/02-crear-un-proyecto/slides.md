---
marp: true
theme: default
paginate: true
---

# Quarkus a Fondo

## Sesión 02
### Crear un proyecto: Maven, extensiones y estructura

---

# En la sesión anterior

Creamos y ejecutamos una aplicación Quarkus.

Ahora vamos a entender:

- qué se generó;
- cómo se organiza;
- cómo se administran sus extensiones.

---

# Crear el proyecto

```powershell
mvn "io.quarkus.platform:quarkus-maven-plugin:3.40.1.1:create" `
    "-DprojectGroupId=io.github.apuntesdejava.quarkusafondo" `
    "-DprojectArtifactId=02-project-structure" `
    "-Dextensions=rest"
```

Tres decisiones visibles:

- identidad del proyecto;
- versión de la plataforma;
- extensiones iniciales.

---

# Estructura generada

```text
02-project-structure/
├── .mvn/
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

No todo necesita estudiarse hoy.

---

# Qué nos interesa ahora

**Ahora**

- `pom.xml`
- Maven Wrapper
- `src/main/java`
- `src/main/resources`

**Más adelante**

- tests
- Dockerfiles
- configuración
- REST
- Dev Mode
- Native Image

---

# Maven Wrapper

```powershell
.\mvnw.cmd -version
```

El proyecto puede indicar qué versión de Maven utilizar.

```text
.mvn/wrapper/maven-wrapper.properties
```

No dependemos únicamente de la instalación global del equipo.

---

# Identidad del proyecto

```xml
<groupId>io.github.apuntesdejava.quarkusafondo</groupId>
<artifactId>02-project-structure</artifactId>
<version>1.0.0-SNAPSHOT</version>
<packaging>quarkus</packaging>
```

Coordenadas Maven + tipo de empaquetado del proyecto.

---

# Plataforma Quarkus

```xml
<quarkus.platform.group-id>
    io.quarkus.platform
</quarkus.platform.group-id>

<quarkus.platform.artifact-id>
    quarkus-bom
</quarkus.platform.artifact-id>

<quarkus.platform.version>
    3.40.1.1
</quarkus.platform.version>
```

Una versión de plataforma coordina el conjunto de componentes Quarkus.

---

# BOM

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

Por eso muchas dependencias Quarkus no declaran una versión propia.

---

# Una dependencia Quarkus

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-rest</artifactId>
</dependency>
```

La versión se obtiene de la plataforma.

---

# Quarkus Maven Plugin

```xml
<plugin>
    <groupId>${quarkus.platform.group-id}</groupId>
    <artifactId>quarkus-maven-plugin</artifactId>
    <version>${quarkus.platform.version}</version>
    <extensions>true</extensions>
</plugin>
```

Nos permite trabajar con el proyecto mediante goals de Maven.

---

# Extensiones instaladas

```powershell
mvn quarkus:list-extensions "-Dinstalled=true"
```

Resultado inicial:

```text
quarkus-rest    REST
```

---

# Agregar una extensión

```powershell
mvn quarkus:add-extension `
    "-Dextensions=smallrye-health"
```

El `pom.xml` recibe:

```xml
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-smallrye-health</artifactId>
</dependency>
```

Sin versión explícita.

---

# Relación visible

```text
extensión Quarkus
       │
       ▼
dependencia Maven
       │
       ▼
versión administrada
por la plataforma
```

---

# Eliminar la extensión

```powershell
mvn quarkus:remove-extension `
    "-Dextensions=smallrye-health"
```

La usamos solo para observar el mecanismo.

La demo vuelve a quedar con REST.

---

# Código generado

```text
src/main/java/
└── io/github/apuntesdejava/quarkusafondo/
    └── GreetingResource.java
```

Hoy nos interesa:

- dónde vive el código;
- cómo se refleja el package.

Las anotaciones REST vendrán después.

---

# Configuración

```text
src/main/resources/application.properties
```

Está vacío en la demo.

Lo usaremos cuando lleguemos a:

- configuración;
- perfiles;
- fuentes de configuración.

---

# También se generan...

```text
src/test/
src/main/docker/
README.md
```

Quarkus prepara piezas para temas posteriores.

Reconocerlas no significa explicarlas todavía.

---

# Issue Tracker

El proyecto transversal ya existe.

Hoy solo verificamos que utiliza la misma base:

```text
pom.xml
Maven Wrapper
src/
Quarkus Platform
BOM
extensiones
```

---

# Extensiones del Issue Tracker

```powershell
mvn quarkus:list-extensions "-Dinstalled=true"
```

```text
quarkus-rest    REST
```

No agregamos funcionalidad nueva en esta sesión.

---

# Resultado

Ahora podemos mirar un proyecto Quarkus y reconocer:

- su identidad Maven;
- la plataforma y el BOM;
- el plugin de Quarkus;
- las extensiones;
- Maven Wrapper;
- la estructura principal.

---

# Siguiente sesión

## Sesión 03
### Dev Mode y ciclo de desarrollo

Ya sabemos qué contiene el proyecto.

Ahora veremos cómo cambia la experiencia mientras desarrollamos.
