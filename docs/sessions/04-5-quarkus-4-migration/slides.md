---
marp: true
theme: default
paginate: true
---

# Sesión 04.5 - Actualizamos Quarkus a Fondo a Quarkus 4

## De Quarkus 3.40.1.1 a 4.0.1

**Quarkus a Fondo**

---

# ¿Qué estamos actualizando?

**Solo las aplicaciones actuales del curso.**

```text
Demo aislada      REST + ArC + /hello
Issue Tracker     REST + ArC + /hello
```

Todavía no hemos incorporado persistencia, seguridad, mensajería ni integraciones externas.

**Este resultado no representa una migración general de Quarkus 3 a 4.**

---

# Una versión mayor merece revisión

Antes de actualizar:

- comprobar la versión y las extensiones utilizadas;
- verificar que la aplicación funciona;
- consultar la guía oficial de migración;
- trabajar sobre un estado recuperable con Git.

**La herramienta ayuda, pero no garantiza compatibilidad.**

---

# Actualización con Maven

Desde la demo aislada con Quarkus 3.40.1.1:

```powershell
mvn io.quarkus.platform:quarkus-maven-plugin:4.0.1:update -N
```

El actualizador:

- propone actualizar el BOM;
- detecta las extensiones gestionadas por la plataforma;
- selecciona recetas de OpenRewrite;
- solicita confirmación para aplicarlas.

**El objetivo `update` es experimental.**

---

# ¿Qué cambió en nuestras aplicaciones?

| Propiedad del `pom.xml` | Antes | Después |
|---|---|---|
| `quarkus.platform.version` | `3.40.1.1` | `4.0.1` |
| `compiler-plugin.version` | `3.15.0` | `3.16.0` |

No hubo cambios en los archivos Java.

**Son los cambios observados en nuestros ejemplos, no una receta universal.**

---

# `BUILD SUCCESS`: ¿qué estamos verificando?

```text
update finaliza
      |
      v
revisar git diff
      |
      v
mvn clean verify
      |
      v
mvn quarkus:dev
      |
      v
consultar /hello
```

El éxito del actualizador no sustituye las pruebas de la aplicación.

---

# Resultados comprobados

| Verificación | Demo | Issue Tracker |
|---|---|---|
| Compilación y empaquetado | Correctos | Correctos |
| Prueba automatizada | 1/1 | 1/1 |
| Dev Mode | Quarkus 4.0.1 | Quarkus 4.0.1 |
| `/hello` | HTTP 200 | HTTP 200 |

Las pruebas de integración de Failsafe **no se ejecutaron**.

Las advertencias observadas no impidieron estas comprobaciones.

---

# Continuamos sobre Quarkus 4

Las sesiones 01 a 04 conservan su código publicado en Quarkus 3.

La actualización del Issue Tracker y la nueva demo quedan en Quarkus 4.0.1.

## Próxima sesión: 05 - Configuración con SmallRye Config

Estudiaremos la configuración sobre la nueva versión, sin adelantar tecnologías que aún no forman parte del curso.

**Documentación oficial:**

- https://quarkus.io/guides/update-quarkus/
- https://github.com/quarkusio/quarkus/wiki/Migration-Guide-4.0
