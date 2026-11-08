
# Informe de Implementación
## Práctica 3: Configuración Básica de Pipeline CI/CD

**Institución:** Instituto Tecnológico de Las Américas (ITLA)  
**Asignatura:** Electiva 1 — Introducción a DevOps  
**Repositorio:** https://github.com/angelomdc/Practica3-Pipeline-CICD

---

## 1. Introducción

El objetivo de esta práctica fue implementar un pipeline de integración y despliegue continuo (CI/CD) utilizando GitHub Actions, con el propósito de automatizar la validación, ejecución de pruebas y publicación de una aplicación web.

Se utilizó una aplicación de gestión de tareas desarrollada con Node.js y Express, a la cual se agregó una nueva funcionalidad y sus respectivas pruebas automatizadas.

## 2. Tecnologías utilizadas

- Node.js: entorno de ejecución de JavaScript.
- Express: framework para desarrollar la API.
- Jest: ejecución de pruebas automatizadas.
- Supertest: validación de endpoints HTTP.
- Git y GitHub: control de versiones.
- GitHub Actions: automatización del pipeline.
- Docker: construcción de la imagen de la aplicación.
- GitHub Pages: publicación del frontend estático.
- Visual Studio Code: entorno de desarrollo.

## 3. Implementación realizada

Se utilizó la plantilla proporcionada por el profesor, que incluía seis pruebas automatizadas y una configuración inicial del pipeline.

Se implementó el endpoint:

DELETE /api/tareas/:id

Esta funcionalidad permite eliminar tareas mediante su identificador.

También se agregaron dos pruebas:

1. Verificar que una tarea existente pueda eliminarse correctamente y que la API devuelva HTTP 200.
2. Verificar que, al intentar eliminar una tarea inexistente, la API devuelva HTTP 404.

## 4. Resultados de las pruebas

Después de implementar la nueva funcionalidad, se ejecutaron las pruebas mediante:

`npm.cmd test`

Los resultados obtenidos fueron:

| Indicador | Resultado |
|---|---|
| Pruebas ejecutadas | 8 |
| Pruebas aprobadas | 8 |
| Cobertura de instrucciones | 95.55% |
| Cobertura de ramas | 85.71% |
| Cobertura de funciones | 100% |
| Cobertura de líneas | 100% |

Estos resultados confirmaron el funcionamiento de los endpoints evaluados.

## 5. Configuración del pipeline CI/CD

Se utilizó GitHub Actions mediante el archivo:

`.github/workflows/ci.yml`

El pipeline se activa automáticamente cuando se realiza un push o pull request hacia la rama main.

Sus etapas principales son:

1. Descargar el código fuente desde GitHub.
2. Configurar el entorno Node.js.
3. Instalar las dependencias.
4. Revisar la sintaxis del código JavaScript.
5. Ejecutar las pruebas automatizadas con cobertura.
6. Construir una imagen Docker.
7. Desplegar el frontend a GitHub Pages si las etapas anteriores se completan correctamente.

La etapa de despliegue se ejecuta únicamente para los push a la rama main que superan las validaciones.

## 6. Demostración de un pipeline fallido

Para comprobar que el pipeline detectaba errores, se modificó intencionalmente una prueba automatizada.

La prueba esperaba una respuesta HTTP 201 cuando el endpoint DELETE devolvía HTTP 200.

Como resultado, GitHub Actions mostró el estado Failure en la etapa de pruebas.

El despliegue a GitHub Pages fue omitido, demostrando que el pipeline evita publicar cambios cuando las validaciones fallan.

## 7. Corrección y recuperación

Posteriormente, se corrigió la prueba para que esperara HTTP 200.

Se realizó un nuevo commit y push hacia GitHub.

GitHub Actions ejecutó nuevamente las etapas de validación y despliegue, finalizando correctamente.

Esto permitió demostrar el funcionamiento del proceso de integración y despliegue continuo.

## 8. Despliegue

El frontend estático fue publicado exitosamente mediante GitHub Pages.

**URL de la aplicación:**

https://angelomdc.github.io/Practica3-Pipeline-CICD/

**Repositorio del proyecto:**

https://github.com/angelomdc/Practica3-Pipeline-CICD

El despliegue corresponde al contenido estático de la carpeta public. La API Express se ejecuta por separado y no está alojada en GitHub Pages.

## 9. Evidencias

Las capturas del proceso se encuentran en la carpeta `docs/evidencias`.

Se documentaron los siguientes resultados:

1. Ejecución inicial de seis pruebas.
2. Fallo de las dos pruebas nuevas.
3. Ejecución exitosa de las ocho pruebas.
4. Pipeline fallido en GitHub Actions.
5. Pipeline recuperado y exitoso.
6. Aplicación publicada en GitHub Pages.

## 10. Conclusión

La realización de esta práctica permitió comprender la importancia de automatizar los procesos de integración, validación y despliegue de aplicaciones.

Se comprobó cómo GitHub Actions puede detectar errores en el código antes de publicar una nueva versión, evitando que cambios incorrectos lleguen al entorno de despliegue.

Además, se fortalecieron los conocimientos relacionados con Git, GitHub, Node.js, pruebas automatizadas, Docker y despliegue continuo.

El resultado final fue un pipeline CI/CD funcional, capaz de ejecutar pruebas, construir una imagen Docker y publicar automáticamente el frontend de la aplicación.
