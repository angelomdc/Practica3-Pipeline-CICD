# Plantilla DevOps — Electiva 1 (ITLA 2026-C-3)

Aplicación web mínima (lista de tareas) con todo lo necesario para practicar un pipeline **CI/CD** sin cuentas pagas ni Azure:

| Pieza | Archivo | Para qué sirve |
|---|---|---|
| App Node.js/Express | `src/app.js`, `src/server.js` | API de tareas + `/health` y `/metrics` |
| Frontend estático | `public/index.html` | Se publica en GitHub Pages |
| Pruebas automatizadas | `tests/app.test.js` | Jest + Supertest con cobertura |
| Contenedor | `Dockerfile` | Imagen de producción con *healthcheck* |
| Pipeline | `.github/workflows/ci.yml` | Lint → pruebas → build Docker → deploy a Pages |

## Requisitos (gratis)
- Cuenta de **GitHub** (usa un repositorio **público**: GitHub Actions es gratuito).
- **Git** y **Node.js 20+**. Opcional para probar el contenedor en tu PC: **Docker Desktop**.

## Ejecutar en tu computadora
```bash
npm install
npm test          # pruebas + cobertura
npm start         # http://localhost:3000
```
Endpoints: `GET /health`, `GET /metrics`, `GET/POST /api/tareas`, `PATCH /api/tareas/:id`.

Con Docker:
```bash
docker build -t tareas-devops .
docker run -p 3000:3000 tareas-devops
```

## Subirlo a GitHub y activar el pipeline
1. Crea un repositorio **público** vacío en GitHub (sin README).
2. En la carpeta de la plantilla:
   ```bash
   git init
   git add .
   git commit -m "Plantilla inicial"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
   git push -u origin main
   ```
3. En GitHub: **Settings → Pages → Source: GitHub Actions**.
4. Abre la pestaña **Actions**: verás el workflow *CI/CD* ejecutándose. Cuando termine, la URL de tu página aparece en el job *Despliegue a GitHub Pages*.

## Práctica 3 — qué debes agregar
- Al menos **2 pruebas nuevas** (por ejemplo: borrar tareas, validar títulos largos).
- Una funcionalidad nueva pequeña con su prueba (ej. `DELETE /api/tareas/:id`).
- Provocar a propósito una prueba fallida, mostrar que el pipeline se pone en **rojo**, corregirla y mostrarlo en **verde**.
- Completar este README con la explicación de cada etapa del pipeline.

## Proyecto Final — cómo extenderla
- Persistencia con **SQLite** (o MongoDB) en lugar del arreglo en memoria.
- Publicar la imagen en **GitHub Container Registry** (`ghcr.io`) desde el pipeline.
- Análisis estático real (ESLint) y umbral de cobertura mínima.
- Monitoreo: usar `/metrics` y los logs JSON; definir al menos una **alerta** (ej. errores > 0).
- Documentar: guía de instalación, manual de operaciones y lecciones aprendidas.


## Práctica 3 — Implementación del Pipeline CI/CD

### Descripción del proyecto

Este proyecto consiste en una aplicación web de gestión de tareas desarrollada con Node.js y Express. Implementa un pipeline CI/CD mediante GitHub Actions para automatizar la validación del código, las pruebas unitarias, la construcción de una imagen Docker y el despliegue a GitHub Pages.

### Funcionalidad implementada

Se agregó el endpoint DELETE /api/tareas/:id, que permite eliminar una tarea mediante su identificador.

- Si la tarea existe, devuelve HTTP 200 y confirma su eliminación.
- Si la tarea no existe, devuelve HTTP 404 con un mensaje de error.

### Pruebas automatizadas

Se utilizaron Jest y Supertest. Se agregaron dos pruebas nuevas:

1. Verificar que se pueda eliminar una tarea existente.
2. Verificar que eliminar una tarea inexistente devuelva HTTP 404.

Resultados finales:

- 8 pruebas aprobadas.
- 95.55% de cobertura de instrucciones.
- 85.71% de cobertura de ramas.
- 100% de cobertura de funciones.
- 100% de cobertura de líneas.

### Etapas del pipeline CI/CD

El workflow está definido en `.github/workflows/ci.yml`.

**1. Activación automática**

El pipeline se ejecuta cuando se realiza un push o se abre o actualiza un pull request hacia la rama main.

**2. Preparación del entorno**

GitHub Actions descarga el código del repositorio y configura Node.js para ejecutar el proyecto.

**3. Instalación de dependencias**

Se ejecuta `npm install` para instalar las dependencias necesarias.

**4. Revisión estática**

Se ejecuta `npm run lint`, que comprueba la sintaxis de los archivos principales de JavaScript.

**5. Pruebas automatizadas**

Se ejecuta `npm test`, utilizando Jest y Supertest para validar los endpoints y generar el reporte de cobertura.

**6. Construcción Docker**

Se ejecuta `docker build` para verificar que la aplicación pueda empaquetarse correctamente en una imagen Docker.

**7. Despliegue a GitHub Pages**

Si las etapas anteriores terminan correctamente y el evento es un push a main, el pipeline publica el contenido de la carpeta `public` en GitHub Pages.

GitHub Pages aloja el frontend estático; no ejecuta el servidor Express.

### Demostración de detección de errores

Se modificó intencionalmente una prueba para que esperara el código HTTP 201 en lugar del 200 devuelto por el endpoint DELETE.

Al publicar el cambio:

- GitHub Actions detectó la prueba fallida.
- La etapa Build y pruebas terminó en rojo.
- El despliegue a GitHub Pages fue omitido.

Posteriormente se corrigió la prueba para esperar HTTP 200. Se realizó un nuevo commit y push, logrando que las pruebas y el despliegue finalizaran correctamente.

Esto demuestra cómo CI/CD permite detectar errores automáticamente antes de publicar una nueva versión.

### Ejecutar el proyecto localmente

Instalar dependencias:

`npm install`

Ejecutar las pruebas:

`npm test`

Iniciar el servidor:

`npm start`

La API estará disponible en:

`http://localhost:3000`

### Guía de contribución

1. Clonar el repositorio.
2. Crear una rama para desarrollar los cambios.
3. Implementar la funcionalidad y sus pruebas.
4. Ejecutar las pruebas localmente.
5. Crear un commit con una descripción clara.
6. Enviar los cambios y crear un pull request hacia main.
7. Verificar que GitHub Actions complete sus validaciones.

### Enlaces del proyecto

Repositorio:
https://github.com/angelomdc/Practica3-Pipeline-CICD

Aplicación desplegada:
https://angelomdc.github.io/Practica3-Pipeline-CICD/

### Conclusión

La práctica permitió implementar y comprobar un pipeline CI/CD automatizado con GitHub Actions. Se demostró la ejecución de pruebas, la detección de errores, la construcción de una imagen Docker y el despliegue continuo de una aplicación web estática.
