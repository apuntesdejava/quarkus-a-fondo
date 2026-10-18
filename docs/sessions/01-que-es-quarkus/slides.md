---
marp: true
theme: default
paginate: true
size: 16:9
---

# Quarkus a Fondo

## 01 — Qué es Quarkus y qué problema intenta resolver

**¿Por qué otro framework Java?**

---

# Objetivo

Al terminar esta sesión:

- ubicar Quarkus en el ecosistema Java;
- entender **build time vs runtime**;
- conocer el papel de las **extensiones**;
- ejecutar nuestra primera aplicación.

---

# El contexto cambió

```text
Aplicación empresarial tradicional

      arrancar
         │
         ▼
┌──────────────────────┐
│ ejecución prolongada │
│ horas · días · meses │
└──────────────────────┘
```

**cloud · contenedores · escalamiento · densidad · scale-to-zero**

---

# Nuevas restricciones

```text
          aplicación
          /       \
         /         \
        ▼           ▼

   tiempo de     consumo de
   arranque       recursos
```

---

# Una pregunta diferente

> Si conocemos parte de la aplicación durante el build...

## ¿tenemos que descubrirla otra vez al arrancar?

---

# Build time y runtime

### Modelo simplificado

```text
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

---

# La dirección de Quarkus

```text
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

**Hacer antes lo que sea razonable hacer antes.**

---

# Quarkus y Java

```text
               Quarkus
                  │
       ┌──────────┼──────────┐
       │          │          │
   Jakarta EE  MicroProfile  Hibernate
       │          │          │
       └──── ecosistema Java ┘
```

**Integra el ecosistema; no parte de cero.**

---

# Extensiones

```text
        Aplicación Quarkus
               │
     ┌─────────┼─────────┐
     │         │         │
    REST   Hibernate   PostgreSQL
     │         │         │
    OIDC     Kafka   OpenTelemetry
```

Las capacidades aparecen **cuando las necesitamos**.

---

# Una extensión Quarkus

## no es solamente...

```xml
<dependency>
    ...
</dependency>
```

También puede integrarse con:

**build · configuración · desarrollo · runtime**

---

# Algunos mitos

| Afirmación | |
|---|:---:|
| Quarkus = Native Image | ❌ |
| Solo sirve para Kubernetes | ❌ |
| Reemplaza Jakarta EE | ❌ |
| Obliga a programar reactivo | ❌ |

---

# Entorno del curso

```text
Java
 +
Maven
  │
  ▼
Quarkus Maven Plugin
```

**Sin Quarkus CLI**

---

# Maven: versión explícita

Si usamos el script de instalación:

```powershell
.\setup-mvn.ps1 -MavenVersion 3.9.16
```

Después:

```powershell
mvn -version
```

**El curso no depende del valor por defecto del script.**

---

# Primera demo

## `demos/01-getting-started/`

```text
crear
  │
  ▼
ejecutar
  │
  ▼
hacer una petición
  │
  ▼
observar la respuesta
```

---

# Crear con Maven

```powershell
mvn io.quarkus.platform:quarkus-maven-plugin:3.40.1:create `
  "-DprojectArtifactId=01-getting-started" `
  "-Dextensions=rest"
```

> Los parámetros completos se explican en la sesión 02.

---

# Ejecutar

```powershell
mvn quarkus:dev
```

Después:

```text
GET /hello
```

🎯 **Objetivo: comprobar que funciona**

---

# Lo vemos, pero todavía no lo estudiamos

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

---

# Dos aplicaciones, dos propósitos

```text
demos/01-getting-started/
         │
         ▼
 experimento aislado
```

```text
issue-tracker/
         │
         ▼
 aplicación transversal
         │
         ▼
 evoluciona sesión a sesión
```

---

# Issue Tracker — Sesión 01

```text
quarkus-a-fondo/
├── docs/
├── demos/
│   └── 01-getting-started/
└── issue-tracker/
```

Por ahora:

**crear · ejecutar · comprobar**

Sin funcionalidad de negocio.

---

# Qué debemos llevarnos

```text
El contexto cambió
        │
        ▼
más trabajo en build time
        │
        ▼
menos trabajo al arrancar
```

**ecosistema Java + extensiones**

---

# Siguiente sesión

## 02 — Crear un proyecto

```text
Maven
  │
plugin de Quarkus
  │
pom.xml
  │
extensiones
  │
estructura
```

**Ahora sí vamos a abrir la caja.**
