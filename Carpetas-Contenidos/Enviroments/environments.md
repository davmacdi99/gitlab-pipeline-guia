# 🌍 Environments en GitLab CI/CD

Los **Environments (Entornos)** en GitLab permiten **visualizar y gestionar dónde se despliega tu aplicación**, facilitando el seguimiento, la auditoría y el control de tus despliegues en diferentes fases del ciclo de vida: desarrollo, staging, producción, etc.

---

## 1. ¿Qué es un environment? 🧭

Un **environment** representa un destino de despliegue, como puede ser:
- Un servidor de pruebas
- Un entorno de staging (pre-producción)
- El entorno de producción real

GitLab registra los despliegues realizados en cada entorno y muestra un panel con:
- Historial de despliegues
- URLs asociadas
- Último commit desplegado

---

## 2. Declarar un environment en un job 🛠️

Para que GitLab reconozca un entorno, debes declararlo en el job que hace el despliegue:

```yaml
deploy_staging:
  stage: deploy
  script:
    - echo "Desplegando en staging..."
  environment:
    name: staging
    url: https://staging.miapp.com
```
-  name: Nombre del entorno (puede ser dev, qa, staging, production...).
-  url: Dirección pública o interna a la que se ha desplegado la app (opcional pero útil).

---

## 3. Environments dinámicos 🧪

Podemos crear entornos por rama o feature usando variables dinámicas:

```yaml
deploy_review:
  stage: deploy
  script:
    - echo "Desplegando revisión para $CI_COMMIT_REF_NAME"
  environment:
    name: review/$CI_COMMIT_REF_NAME
    url: https://review-$CI_COMMIT_REF_SLUG.miapp.com
```
🎯 Útil para generar entornos temporales para revisar cambios antes de hacer merge.

---

## 4. Monitorización del entorno 📊

Una vez definidos los environments:

- GitLab muestra el estado de cada uno en la pestaña Deployments.

- Podemos ver cuándo se desplegó, quién lo hizo y con qué commit.

- También podemos detener entornos dinámicos si ya no son necesarios (ahorrando recursos).

---

## 5. Detener un environment ⛔

```yaml
stop_review:
  stage: cleanup
  script:
    - echo "Eliminando entorno dinámico"
  environment:
    name: review/$CI_COMMIT_REF_NAME
    action: stop
```
📌 Esto indica a GitLab que ese entorno ha sido desmontado o eliminado.

---

## 6. Ejemplo completo 🎯
```yaml
stages:
  - build
  - deploy
  - cleanup

build_app:
  stage: build
  script:
    - echo "Construyendo app..."

deploy_staging:
  stage: deploy
  script:
    - echo "Deploy a staging"
  environment:
    name: staging
    url: https://staging.miapp.com
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'

deploy_production:
  stage: deploy
  script:
    - echo "Deploy a producción"
  when: manual
  environment:
    name: production
    url: https://miapp.com
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'

stop_review:
  stage: cleanup
  script:
    - echo "Borrando entorno de revisión"
  environment:
    name: review/$CI_COMMIT_REF_NAME
    action: stop
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### 🔍 Explicación del ejemplo
Este pipeline define tres etapas principales: build, deploy y cleanup. Cada job está vinculado a una etapa y tiene reglas específicas para controlar cuándo y cómo se ejecuta.

🧱 build_app

- Este job se encarga de simular la construcción (build) de la aplicación.

- Pertenece a la etapa build.

- Ejecuta un comando sencillo como ejemplo, pero aquí irían tareas como compilar el código o generar binarios.

🚀 deploy_staging

- Despliega la aplicación en el entorno de staging.

- Solo se ejecuta cuando el commit pertenece a la rama develop.

- Define un environment llamado staging con una URL donde supuestamente se despliega la app.

- Muy útil para probar antes de subir a producción.

🚢 deploy_production

- Este job despliega la app en producción.

- Se ejecuta manualmente (when: manual) desde la interfaz de GitLab, como medida de seguridad.

- Solo está disponible si el commit proviene de la rama main.

- El entorno definido se llama production, con su correspondiente URL pública.

🧹 stop_review

- Este job se encarga de eliminar entornos de revisión temporales.

- El nombre del environment se genera dinámicamente con el nombre de la rama o MR.

- Se activa únicamente cuando el pipeline ha sido lanzado desde un evento de Merge Request.

- El parámetro action: stop indica que el entorno será detenido/eliminado.
