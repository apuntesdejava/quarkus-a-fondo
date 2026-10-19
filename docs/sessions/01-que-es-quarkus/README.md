# Sesión 01 — Qué es Quarkus y qué problema intenta resolver

> Estado: borrador de contenido para revisión.

## Objetivo de aprendizaje

Al terminar la sesión, el estudiante debería poder explicar qué es Quarkus, por qué surge dentro del ecosistema Java moderno, qué diferencia conceptual existe entre trabajo realizado en build time y en runtime, qué papel cumplen las extensiones y cómo ejecutar por primera vez una aplicación Quarkus.

La sesión debe responder principalmente a una pregunta:

**¿Por qué existe Quarkus?**

No se pretende explicar todavía en profundidad Dev Mode, CDI/ArC, programación reactiva, contenedores, Kubernetes, GraalVM Native Image ni los mecanismos internos de augmentation. Esos temas aparecen posteriormente en el roadmap.

## Entorno base

La sesión utiliza el baseline actual del curso:

- JDK 25;
- Apache Maven 3.9.16;
- Git.

Maven es la herramienta principal para crear y ejecutar los proyectos. La Quarkus CLI no forma parte de los requisitos del curso.

Los comandos reproducibles de preparación, creación, ejecución y verificación están en [`commands.md`](commands.md).

## El problema que intenta resolver

Java dispone de un ecosistema empresarial maduro, con estándares, bibliotecas y frameworks capaces de resolver una enorme variedad de problemas. Buena parte de ese ecosistema nació y evolucionó en un contexto donde una aplicación podía arrancar una vez y permanecer ejecutándose durante largos periodos.

En aplicaciones tradicionales, parte importante de la preparación del framework puede realizarse durante el arranque: descubrir componentes, procesar metadatos, leer configuración, crear proxies o preparar estructuras internas. Ese modelo sigue siendo válido, pero los entornos de ejecución modernos introducen otras restricciones.

Contenedores, plataformas cloud, escalamiento dinámico, alta densidad de aplicaciones y modelos como scale-to-zero hacen que el tiempo de arranque y el consumo de memoria tengan más importancia que en muchos despliegues tradicionales.

Quarkus parte de una idea central: si parte del trabajo necesario para preparar una aplicación puede conocerse durante la construcción del artefacto, no siempre es necesario repetirlo cada vez que la aplicación arranca.

Esta sesión introduce esa idea sin entrar todavía en sus mecanismos internos.

## Qué es Quarkus

Quarkus es un framework y stack Java orientado a la construcción de aplicaciones modernas, especialmente adecuado para entornos cloud y contenedores, pero no limitado a ellos.

Integra tecnologías y estándares conocidos del ecosistema Java, entre ellos Jakarta EE, MicroProfile, Hibernate ORM, SmallRye y Vert.x.

A lo largo del curso será importante distinguir entre tres niveles:

- los estándares o APIs que utilizamos;
- las implementaciones concretas;
- la integración y optimización que Quarkus realiza sobre ellas.

En esta primera sesión basta con comprender que Quarkus intenta conservar las ventajas del ecosistema Java y adaptar el modelo de ejecución y la experiencia de desarrollo a las necesidades actuales.

## Build time y runtime: la intuición inicial

Una de las ideas fundamentales de Quarkus es realizar durante el build tanto trabajo como sea razonablemente posible.

De forma simplificada:

```text
Enfoque tradicional

build
  |
  v
artefacto
  |
  v
arranque
  |
  +-- preparación del framework
  +-- descubrimiento de componentes
  +-- procesamiento de metadatos
  +-- inicialización
  |
  v
aplicación lista
```

Quarkus intenta desplazar parte de esa preparación:

```text
Quarkus

build
  |
  +-- análisis y preparación
  |
  v
artefacto preparado
  |
  v
arranque
  |
  v
aplicación lista
```

Este dibujo es deliberadamente conceptual. Más adelante, en la ruta avanzada, se estudiarán augmentation, build steps, build items, class loading y otros mecanismos que explican cómo se consigue.

Lo importante en esta sesión es comprender la consecuencia: hacer más trabajo antes del arranque permite reducir trabajo posterior y puede contribuir a mejorar tiempo de inicio y consumo de recursos.

Estas optimizaciones no dependen exclusivamente de compilar una aplicación como ejecutable nativo. Quarkus también aplica su modelo de optimización cuando la aplicación se ejecuta sobre la JVM.

## Filosofía de extensiones

Quarkus organiza gran parte de sus capacidades mediante extensiones.

Una aplicación puede incorporar, por ejemplo, capacidades para REST, persistencia, bases de datos, seguridad, mensajería u observabilidad. A lo largo del curso iremos agregando estas capacidades únicamente cuando exista una necesidad que aprender y resolver.

```text
Aplicación Quarkus
      |
      +-- REST
      +-- Hibernate ORM
      +-- PostgreSQL
      +-- OIDC
      +-- OpenTelemetry
      +-- ...
```

En esta sesión no se estudiará todavía cómo agregar, eliminar o administrar extensiones. Eso pertenece a la sesión 02.

Una extensión Quarkus tampoco debe entenderse simplemente como otra dependencia Maven. Las extensiones permiten integrar una tecnología con el modelo de construcción, configuración, desarrollo y ejecución de Quarkus. Los detalles técnicos de esa integración se aprenderán progresivamente.

## Alcance de esta sesión

La primera sesión debe evitar convertir la introducción en un catálogo de características. Algunos conceptos pueden mencionarse para ubicar Quarkus, pero su explicación corresponde a sesiones posteriores.

En particular:

- **Dev Mode** tendrá una sesión propia.
- **Dev UI** tendrá una sesión propia.
- **CDI y ArC** comenzarán cuando estudiemos beans, scopes e inyección.
- **programación reactiva, blocking/non-blocking y Mutiny** aparecerán en el bloque de modelos de ejecución.
- **contenedores y Kubernetes** se estudiarán en el bloque de producción.
- **Native Image** se analizará cuando ya conozcamos empaquetado, contenedores y operación.
- **augmentation e internals de Quarkus** pertenecen a la ruta avanzada.

## Malentendidos frecuentes

Quarkus puede ejecutarse sobre la JVM o compilarse posteriormente como ejecutable nativo. Native Image es una opción de empaquetado y despliegue, no un requisito para utilizar el framework.

Su diseño presta especial atención a entornos cloud y contenedores, pero varias de sus optimizaciones también son útiles fuera de Kubernetes.

El framework se apoya en estándares y bibliotecas del ecosistema Java, por lo que aprender Quarkus no implica sustituir Jakarta EE, MicroProfile, Hibernate u otras tecnologías que aparecerán durante el curso.

Quarkus también admite distintos modelos de programación. La programación reactiva se estudiará cuando exista contexto suficiente para entender sus beneficios y costes.

## Demo aislada: `demos/01-getting-started/`

La primera demo debe ser deliberadamente pequeña. Su objetivo no es enseñar todavía la estructura completa de un proyecto Quarkus, sino comprobar que podemos crear y ejecutar una aplicación.

El recorrido práctico será:

```text
crear
  |
  v
abrir
  |
  v
ejecutar
  |
  v
hacer una petición HTTP
  |
  v
observar la respuesta
```

Durante la demostración pueden señalarse elementos que se estudiarán después: el `pom.xml`, el código Java, la configuración, las extensiones y las pruebas generadas. No se explicará todavía cada uno de esos elementos.

## Verificación de la demo

La comprobación de esta primera sesión será manual: arrancar la aplicación y realizar una petición HTTP que confirme que responde.

Los tests que genere Quarkus pueden conservarse en el proyecto, pero no es necesario explicarlos todavía. Testing tendrá su bloque formal y, a partir de allí, acompañará al resto del curso.

Esta verificación manual no sustituye la futura estrategia de testing; evita introducir antes de tiempo APIs y herramientas que todavía no han sido enseñadas.

## Aplicación transversal: Issue Tracker

La primera evolución del Issue Tracker consiste únicamente en crear el proyecto base en:

```text
issue-tracker/
```

y comprobar que puede ejecutarse.

No se introducirán todavía Projects, Issues, endpoints de negocio, separación por capas, persistencia, seguridad, usuarios ni ninguna otra capacidad funcional.

El resultado esperado al terminar la sesión es:

```text
quarkus-a-fondo/
├── demos/
│   └── 01-getting-started/
└── issue-tracker/
```

Ambas aplicaciones deben poder ejecutarse.

En esta sesión la demo y el Issue Tracker pueden parecer casi idénticos. La diferencia está en su propósito: `demos/01-getting-started/` queda como laboratorio aislado de esta sesión, mientras que `issue-tracker/` conserva su historia y evoluciona progresivamente.

## Resultado final de la sesión

Al finalizar:

- existe una primera demo Quarkus ejecutable en `demos/01-getting-started/`;
- existe el proyecto inicial del Issue Tracker;
- el estudiante puede explicar a alto nivel qué problema intenta resolver Quarkus;
- distingue build time de runtime de forma introductoria;
- entiende que las capacidades de Quarkus se incorporan mediante extensiones;
- sabe que Quarkus puede ejecutarse sobre la JVM y que Native Image es una posibilidad posterior, no un requisito.

## Conexión con la sesión 02

La primera sesión utiliza el proyecto sin desmontar todavía todas sus piezas.

La siguiente sesión responderá preguntas que quedan deliberadamente abiertas:

- ¿qué generó exactamente el comando de creación?;
- ¿qué papel tienen Maven y el plugin de Quarkus?;
- ¿qué contiene el `pom.xml`?;
- ¿cómo se agregan o eliminan extensiones?;
- ¿qué estructura tiene un proyecto Quarkus?;
- ¿qué archivos conviene conocer desde el principio?

Esto conduce directamente a:

**Sesión 02 — Crear un proyecto: Maven, extensiones y estructura.**

## Fuentes oficiales

Fuentes principales para preparar y verificar esta sesión:

- Quarkus — Get Started: https://quarkus.io/get-started/
- Quarkus — Getting Started Guide: https://quarkus.io/guides/getting-started
- Quarkus — About: https://quarkus.io/about/
- Quarkus — Container First: https://quarkus.io/container-first/

Fuentes complementarias para verificar afirmaciones introductorias que se desarrollarán en sesiones posteriores:

- Quarkus — Building a Native Executable: https://quarkus.io/guides/building-native-image
- Quarkus — Reactive Architecture: https://quarkus.io/guides/quarkus-reactive-architecture
- Quarkus — Writing Extensions: https://quarkus.io/guides/writing-extensions
