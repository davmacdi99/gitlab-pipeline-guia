# Uso de Variables en GitLab CI/CD 🔧

Las **variables** en GitLab CI/CD permiten almacenar valores reutilizables que puedes utilizar en diferentes partes de los jobs y pipelines. Son una herramienta poderosa para hacer nuestros pipelines más dinámicos, seguros y fáciles de mantener.

---

## 1. ¿Qué son las variables en CI/CD? 💡

Las variables son pares clave-valor que se pueden usar para almacenar rutas, tokens, configuraciones, credenciales o cualquier dato que necesitemos reutilizar a lo largo del pipeline.  
GitLab soporta tanto variables **definidas por el usuario** como **variables predefinidas del sistema**.

---

## 2. Tipos de variables 🗂️

### 🔹 Variables globales en el `.gitlab-ci.yml`

Podemos definir variables directamente dentro del archivo YAML para que estén disponibles en **todos los jobs**:

```yaml
variables:
  APP_ENV: production
  DEPLOY_DIR: /var/www/html
```

## 🔹 Variables en el ámbito de un job específico

También podemos definir variables que solo afecten a un job en concreto:

```yaml
deploy_job:
  stage: deploy
  variables:
    DEPLOY_DIR: /home/proyecto
  script:
    - echo "Desplegando en $DEPLOY_DIR"
```

## 🔹 Variables definidas en GitLab (interfaz web)
Puedes definir variables desde la interfaz web de GitLab:

Settings > CI / CD > Variables

Estas variables son ideales para datos sensibles y privados, como:

🔐 Tokens de acceso

🔐 Contraseñas de bases de datos

🔐 Claves de despliegue

Estas variables no se almacenan en el repositorio y son accesibles durante la ejecución del pipeline.

## 🔹 Variables predefinidas de GitLab
GitLab proporciona muchas variables de sistema que están disponibles automáticamente durante la ejecución del pipeline. Algunas comunes son:

| Variable              | Descripción                              |
|------------------------|-------------------------------------------|
| **CI_COMMIT_BRANCH**  | Nombre de la rama del commit              |
| **CI_JOB_STAGE**      | Nombre de la etapa que se está ejecutando |
| **CI_PIPELINE_ID**    | ID del pipeline actual                    |
| **CI_PROJECT_NAME**   | Nombre del proyecto                       |


```yaml
stages:
  - info

show_branch:
  stage: info
  script:
    - echo "📌 Estoy en la rama: $CI_COMMIT_BRANCH"
    # Esta variable indica la rama actual desde la que se ejecuta el pipeline

show_stage:
  stage: info
  script:
    - echo "🎯 Etapa actual del job: $CI_JOB_STAGE"
    # Muestra la stage (por ejemplo: build, test, deploy)

show_pipeline_id:
  stage: info
  script:
    - echo "🆔 ID del pipeline: $CI_PIPELINE_ID"
    # Útil para hacer tracking, logging o debugging

show_project_name:
  stage: info
  script:
    - echo "📁 Nombre del proyecto: $CI_PROJECT_NAME"
    # Muestra el nombre del proyecto actual del repositorio
```
💡 Podemos combinar estas variables para nombrar archivos, etiquetar builds o generar rutas únicas en tus pipelines, por ejemplo:

```yaml
artifacts_job:
  stage: info
  script:
    - mkdir -p "builds/$CI_PROJECT_NAME-$CI_COMMIT_BRANCH"
    - echo "Guardando artefactos en builds/$CI_PROJECT_NAME-$CI_COMMIT_BRANCH"
```

