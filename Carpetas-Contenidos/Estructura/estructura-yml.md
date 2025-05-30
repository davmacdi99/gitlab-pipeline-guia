# Estructura de un `.gitlab-ci.yml`

El archivo `.gitlab-ci.yml` es el corazón de la configuración de GitLab CI/CD. En él se definen los trabajos, las etapas (stages), las condiciones y todo lo necesario para que GitLab ejecute tus pipelines de forma automática.

---

## 1. ¿Qué es un pipeline? 🔄

Un **pipeline** es una serie de pasos o tareas automáticas que se ejecutan en orden para construir, probar y desplegar un proyecto.  
Imaginemos que cada vez que hacemos un cambio en el código, queremos que se compile, se ejecute una serie de pruebas y luego se despliegue automáticamente sin tener que hacerlo manualmente. Eso es lo que hace un pipeline: automatiza todo ese proceso para que sea rápido, confiable y repetible.

---

## 2. ¿Qué es un `.gitlab-ci.yml`? 📄

Es un archivo YAML que se sitúa en la raíz de tu repositorio. Contiene la definición de las **pipelines** que GitLab ejecuta cada vez que hay un cambio en el repositorio (como un push o merge request).

---

## 3. Estructura básica 🏗️

Un pipeline se compone principalmente de:

- **Stages (etapas):** Fases que definen el flujo de trabajo (por ejemplo: build, test, deploy).
- **Jobs (trabajos):** Tareas individuales que se ejecutan dentro de las stages.
- **Scripts:** Comandos que se ejecutan en cada job.
- **Variables, reglas, artefactos, caché:** Opciones avanzadas que controlan comportamiento, reutilización y resultados.

---

## 4. Ejemplo básico ⚙️

```yaml
stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - echo "Compilando la aplicación..."

test_job:
  stage: test
  script:
    - echo "Ejecutando pruebas..."

deploy_job:
  stage: deploy
  script:
    - echo "Desplegando en producción..."
```
## Explicación del ejemplo 📝
- **stages:** Define tres etapas: build, test y deploy. Los jobs se ejecutan en orden según estas etapas.

- **build_job:** Es el trabajo asignado a la etapa build. Ejecuta el script que compila la aplicación.

- **test_job:** Trabajo para ejecutar pruebas dentro de la etapa test.

- **deploy_job:** Trabajo encargado de desplegar la aplicación en la etapa deploy.

## 6. Reglas básicas de configuración 📋
- **El archivo debe tener una sintaxis YAML válida.**

- **Cada job debe tener un nombre único (por ejemplo: build_job).**

- **Cada job debe estar asignado a una etapa.**

- **El orden de las stages determina la ejecución del pipeline.**

- **Los jobs dentro de una misma stage se ejecutan en paralelo, si hay runners disponibles.**


