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

- ubicar Quarkus en el ecosistema Java;
- entender **build time vs runtime**;
- conocer el papel de las **extensiones**;
- ejecutar una primera aplicación.

---

# El contexto cambió

```text
Aplicación empresarial

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

**Integración sobre tecnologías conocidas**

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

# Malentendidos frecuentes

**Native Image** → opción posterior de empaquetado

**Kubernetes** → contexto importante, no requisito

**Jakarta EE** → parte del ecosistema utilizado

**reactividad** → modelo que estudiaremos más adelante

---

# Entorno del curso

```text
JDK 25
  +
Maven 3.9.16
  +
Git
```

**Maven es la herramienta principal**

---

# Preparación reproducible

```text
Gist Java  ──► setup-java.ps1
Gist Maven ──► setup-mvn.ps1
```

Descargar · ejecutar · verificar

**La versión de Maven se indica explícitamente.**

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

# Maven y Quarkus

```text
Maven
  │
  ▼
Quarkus Maven Plugin
  │
  ▼
proyecto
```

Los comandos completos están en **commands.md**.

---

# Lo vemos, pero todavía no lo estudiamos

```text
pom.xml / plugin / extensiones / estructura  → 02
Dev Mode                                     → 03
Dev UI                                       → 04
configuración                                → 05
CDI / ArC                                    → 07
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

**crear · ejecutar · comprobar**

Sin funcionalidad de negocio.

---

# Qué debemos llevarnos

```text
contexto moderno
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
