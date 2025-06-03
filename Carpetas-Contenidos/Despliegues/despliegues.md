# 🚀 Despliegues Manuales y Automáticos en GitLab CI/CD

Una de las mayores ventajas de usar GitLab CI/CD es automatizar el proceso de despliegue, permitiéndote lanzar versiones de forma más rápida, segura y controlada.

En esta guía aprenderemos a crear tanto despliegues automáticos como manuales, con ejemplos prácticos.

---

## 1. ¿Qué es un despliegue? 📦

Un **despliegue (deploy)** es el proceso de trasladar nuestro código o aplicación a un entorno de ejecución real, como `staging`, `producción`, o un servidor de pruebas.

---

## 2. Tipos de despliegue 🛠️

| Tipo        | ¿Cuándo se ejecuta?                              | Ideal para...                        |
|-------------|---------------------------------------------------|--------------------------------------|
| **Automático** | Se ejecuta automáticamente tras pasar etapas previas | Entornos de staging, desarrollo      |
| **Manual**     | Requiere intervención desde la interfaz web     | Producción, despliegues controlados  |

---

## 3. Despliegue Automático ⚙️

```yaml
deploy_staging:
  stage: deploy
  script:
    - echo "Desplegando a staging..."
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'
```
🎯 Este job se ejecutará automáticamente solo si estamos en la rama develop.

---

## 4. Despliegue Manual 🧑‍💻
```yaml
deploy_production:
  stage: deploy
  script:
    - echo "Desplegando a producción..."
  when: manual
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```
🛑 Este job no se ejecuta automáticamente. Aparecerá un botón en la interfaz de GitLab para lanzarlo manualmente, solo si estamos en main.

---

## 5. Combinando ambos 🎯

```yaml
stages:
  - build
  - test
  - deploy

deploy_staging:
  stage: deploy
  script:
    - echo "Deploy automático a staging"
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'

deploy_production:
  stage: deploy
  script:
    - echo "Deploy manual a producción"
  when: manual
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

📌 Así configuramos un flujo realista:

- develop → deploy automático a staging

- main → despliegue manual a producción

---

## 6. Agregando environments 🌍

```yaml
deploy_staging:
  stage: deploy
  environment:
    name: staging
    url: https://staging.midominio.com
  script:
    - echo "Desplegando en staging..."
```
💡 Usar environment permite que GitLab muestre una vista de entornos con historial de despliegues, URLs y mucho más.

---

## 7. Ejemplo completo 🧪
```yaml
stages:
  - build
  - deploy

build_app:
  stage: build
  script:
    - echo "Construyendo aplicación..."

deploy_staging:
  stage: deploy
  script:
    - echo "Desplegando en staging..."
  environment:
    name: staging
    url: https://staging.miapp.com
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'

deploy_production:
  stage: deploy
  script:
    - echo "Desplegando en producción..."
  environment:
    name: production
    url: https://miapp.com
  when: manual
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```

### 🌐 deploy_staging — Despliegue automático a staging

Qué hace:
- stage: Pertenece a la etapa de despliegue (deploy).

- script: Simula un despliegue a un servidor de staging.

- environment:

  - name: Etiqueta el entorno como "staging".

  - url: URL del entorno que se mostrará en GitLab.

- rules:

  - Este job solo se ejecutará si el commit proviene de la rama develop.

  - Por lo tanto, es un despliegue automático, pero condicionado a una rama específica.
  
 
### 🚀 deploy_production — Despliegue manual a producción

Qué hace:
- stage: También en la etapa deploy.

- script: Simula despliegue a producción.

- environment:

  - name: Define este entorno como "production".

  - url: URL pública del entorno de producción.

- when: manual

  - ⚠️ El job no se ejecuta automáticamente.

  - Aparecerá un botón en la interfaz de GitLab que un se debe pulsar si quisieramos.

- rules:

  - Este despliegue solo estará disponible si el commit proviene de la rama main.

  - Ayuda a evitar que cualquier rama despliegue en producción por error.
