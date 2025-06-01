# Condiciones y Reglas en GitLab CI/CD ⚙️

En GitLab CI/CD, las **condiciones y reglas** permiten controlar cuándo y cómo se ejecutan los jobs dentro de un pipeline. Esto nos ayuda a optimizar recursos, evitar ejecuciones innecesarias y adaptar el flujo según las circunstancias.

---

## 1. ¿Qué son las condiciones y reglas? 🤔

Son instrucciones que definen las **situaciones específicas** en las que un job debe ejecutarse o no. Por ejemplo, podemos ejecutar un job solo si el commit es en la rama `main`, o evitar que se ejecute en ramas de desarrollo.

---

## 2. Tipos de condiciones más usados 🛠️

### 🔹 `only` y `except` (antiguo método)

- `only`: Ejecuta el job solo si se cumple la condición (por ejemplo, ramas, tags, pipelines).
- `except`: Ejecuta el job en todos los casos **excepto** si se cumple la condición.

```yaml
test_job:
  script: echo "Ejecutando pruebas"
  only:
    - main
    - tags

deploy_job:
  script: echo "Despliegue automático"
  except:
    - develop
```
⚠️ GitLab recomienda usar rules para mayor flexibilidad.

### 🔹 rules (recomendado)
Es la forma más poderosa y flexible para definir cuándo ejecutar un job. Permite múltiples condiciones y acciones (como permitir que el job se ejecute, se salte o se detenga).

```yaml
deploy_job:
  script: echo "Desplegando..."
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: always
    - if: '$CI_COMMIT_BRANCH =~ /^release\/.*$/'
      when: manual
    - when: never
```

### Explicación del ejemplo con rules 📝
> El job se ejecuta automáticamente si la rama es main.

> Si la rama empieza por release/, el despliegue es manual (requiere que un usuario lo active).

> En cualquier otro caso, el job no se ejecuta.

## Otras opciones en rules ⚙️
> when: Define la acción que puede ser on_success (por defecto), always, manual, delayed, o never.

> changes: Ejecuta el job solo si ciertos archivos han cambiado.

```yaml
lint_job:
  script: echo "Chequeando código"
  rules:
    - changes:
        - "*.js"
        - "*.css"
      when: always
    - when: never
```
## Buenas Prácticas

Buenas prácticas ✅
- Prefiere usar rules en lugar de only y except.

- Usa expresiones claras para evitar ejecuciones innecesarias.

- Combinar rules con variables para mayor control (por ejemplo, if: '$CI_PIPELINE_SOURCE == "merge_request_event"').

- Documentar bien las reglas para facilitar mantenimiento.

  ## Ejemplo completo 🎯

  ```yaml
  stages:
  - build
  - test
  - deploy

  build_job:
  stage: build
  script:
    - echo "Construyendo la aplicación"

  test_job:
  stage: test
  script:
    - echo "Ejecutando tests"
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'
      when: always
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: always
    - when: never

  deploy_job:
  stage: deploy
  script:
    - echo "Desplegando en producción"
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: manual
    - when: never

  ```
## Explicación Ejemplo
### 🧱 Definición de etapas (stages)
```yaml
stages:
  - build
  - test
  - deploy
```
Aquí se definen las tres etapas del pipeline, en el orden en que se deben ejecutar:

1- **build** – construcción o compilación del proyecto.

2- **test** – pruebas del código.

3- **deploy** – despliegue a producción (u otro entorno).

GitLab ejecuta los jobs en el orden de las etapas, de arriba hacia abajo, y dentro de cada etapa, los jobs se ejecutan en paralelo (si hay runners disponibles).

### 🔨 build_job
```yaml
build_job:
  stage: build
  script:
    - echo "Construyendo la aplicación"
```
- Este job se ejecuta en la etapa build.

- Ejecuta un simple comando: muestra el mensaje "Construyendo la aplicación".

- No tiene condiciones (rules), por lo que se ejecutará siempre.
  
#### 🧪 test_job con rules

```yaml
test_job:
  stage: test
  script:
    - echo "Ejecutando tests"
  rules:
    - if: '$CI_COMMIT_BRANCH == "develop"'
      when: always
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: always
    - when: never
```

Este job se ejecuta solo en ramas específicas gracias a las rules.

- ✅ Se ejecuta si la rama es:

  - develop

   - main

❌ No se ejecuta en ninguna otra rama (when: never como fallback final).

💡 Esto es útil si solo queremos que las pruebas se ejecuten en ramas clave como desarrollo o producción.

#### 🚀 deploy_job con ejecución manual
```yaml
deploy_job:
  stage: deploy
  script:
    - echo "Desplegando en producción"
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: manual
    - when: never
```
- El job se ejecuta en la etapa deploy.

- Tiene rules para controlar cuándo y cómo se ejecuta:

    - ✅ Solo aparece si estamos en la rama main.

    - ⚙️ Es manual, es decir, alguien debe hacer clic en “Play” en la interfaz de GitLab para que se ejecute.

- ❌ En cualquier otra rama, no aparece ni se ejecuta.

✅ Este patrón es muy común para proteger el despliegue a producción: solo en main, y con activación manual.





  
