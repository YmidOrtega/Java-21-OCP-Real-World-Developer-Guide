# Java 21 OCP Real-World Developer Guide

Notas de estudio en español para preparar el examen **Oracle Certified Professional: Java SE 21 Developer (1Z0-830)**, con explicaciones, ejemplos y una orientación práctica al desarrollo del día a día.

## 📘 Basado en

Este proyecto nace de la lectura y el estudio del libro:

> **OCP Oracle Certified Professional Java SE 21 Developer Study Guide: Exam 1Z0-830**
> *Jeanne Boyarsky y Scott Selikoff* — Sybex (Wiley)

La estructura de capítulos sigue la del libro. Estas notas son un resumen personal y **no sustituyen al libro**: si te resultan útiles, te recomiendo adquirirlo para tener el material completo y apoyar a sus autores.

## 📚 Contenidos

El proyecto está organizado en una introducción y **14 capítulos**:

| #  | Capítulo                           | Temas principales                                              | Estado |
|----|------------------------------------|----------------------------------------------------------------|:------:|
| —  | Introducción                       | El examen, estrategias de estudio, mapa de objetivos, evaluación inicial | ✅ |
| 1  | Bloques de Construcción            | Estructura de clases, paquetes e imports, tipos de datos, variables y alcance | ✅ |
| 2  | Operadores                         | Operadores unarios, binarios, de asignación y comparación, ternario | ✅ |
| 3  | Toma de decisiones                 | `if`, `switch` y pattern matching, bucles, control de flujo    | ✅ |
| 4  | API principales                    | `String`, `StringBuilder`, arrays, `Math`, fechas y horas      | ✅ |
| 5  | Métodos                            | Diseño de métodos, modificadores de acceso, `static`, varargs, sobrecarga | ✅ |
| 6  | Diseño de clases                   | Herencia, constructores, inicialización, clases abstractas, inmutabilidad | ✅ |
| 7  | Más allá de las clases             | Interfaces, enums, sealed classes, records, clases anidadas, polimorfismo | ✅ |
| 8  | Lambdas e Interfaces Funcionales   | Lambdas, referencias a métodos, interfaces funcionales integradas | ✅ |
| 9  | Colecciones y Genéricos            | `List`, `Set`, `Queue`, `Map`, ordenación, colecciones secuenciadas, genéricos | ✅ |
| 10 | Streams                            | `Optional`, Stream API, streams primitivos, pipelines avanzados | ✅ |
| 11 | Excepciones y Localización         | Manejo de excepciones, recursos, formato de valores, internacionalización | ⏳ |
| 12 | Módulos                            | Programas modulares, declaraciones de módulos, servicios, migración | ⏳ |
| 13 | Concurrencia                       | Hilos, Concurrency API, código thread-safe, streams paralelos  | ⏳ |
| 14 | I/O                                | Archivos y directorios, streams de I/O, NIO.2, serialización   | ⏳ |

Cada capítulo incluye sus secciones teóricas, un **resumen**, los **aspectos esenciales del examen**, **preguntas de repaso** y sus **respuestas**.

## 📝 Estructura del repositorio

```
Java-21-OCP-Real-World-Developer-Guide/
├── src/
│   ├── Indice/          # Una nota por capítulo con enlaces a sus secciones
│   └── Contenido/       # Notas de cada capítulo (con sus imágenes)
│       ├── Introducción/
│       ├── Capítulo 1/
│       └── ...
└── README.md
```

## 💡 Recomendación: usar Obsidian

Las notas están escritas en Markdown con enlaces de [Obsidian](https://obsidian.md/) (`[[...]]`), así que se aprovechan mejor desde ahí:

1. Descarga [Obsidian](https://obsidian.md/).
2. Abre la carpeta `src/` como bóveda.
3. Empieza por las notas de `Indice/` y navega a cada sección.

En GitHub también se pueden leer, aunque los enlaces internos no serán clicables.

## 🚀 Cómo usar este proyecto

1. Sigue los capítulos en orden, empezando por la introducción.
2. Escribe y ejecuta los fragmentos de código en tu IDE para comprobar el comportamiento.
3. Responde las preguntas de repaso antes de mirar las respuestas.
4. Repasa el resumen y los aspectos esenciales antes del examen.

**Requisitos:** [JDK 21](https://www.oracle.com/java/technologies/downloads/#java21), un IDE (IntelliJ IDEA, Eclipse o VS Code) y conocimientos básicos de programación.

## 🎓 Sobre el examen 1Z0-830

- **Certificación:** Oracle Certified Professional: Java SE 21 Developer
- **Duración:** 120 minutos
- **Preguntas:** 50 (opción múltiple)
- **Puntuación mínima:** 68 %

Los datos pueden cambiar; consulta siempre la [página oficial del examen en Oracle University](https://education.oracle.com/) para información actualizada, precio y objetivos.

## 💬 Contribuciones

Si encuentras errores o tienes sugerencias, abre un *issue* o un *pull request*.

## ⚖️ Aviso

Este es un proyecto personal de estudio, sin ánimo de lucro, y **no está afiliado ni respaldado** por Oracle, Sybex/Wiley ni los autores del libro. El contenido original del libro *OCP Oracle Certified Professional Java SE 21 Developer Study Guide* es propiedad de sus autores y su editorial. Oracle y Java son marcas registradas de Oracle y/o sus filiales.

---

<div align="center">

**Hecho con ☕ por [Ymid Ortega](https://github.com/YmidOrtega)**

[![GitHub](https://img.shields.io/badge/GitHub-YmidOrtega-181717?logo=github)](https://github.com/YmidOrtega)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?logo=linkedin)](https://linkedin.com/in/ymidortega)

*Si este proyecto te resulta útil, ¡considera darle una ⭐!*

</div>
