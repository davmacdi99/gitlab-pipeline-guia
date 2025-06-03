# 🧩 Plantillas y Reutilización en GitLab CI/CD

En proyectos grandes o con múltiples repositorios, repetir configuraciones de pipelines puede volverse tedioso. Por suerte, GitLab CI/CD permite **reutilizar código mediante plantillas y archivos incluidos**, lo que hace tu pipeline más limpio, mantenible y DRY (Don't Repeat Yourself).

---

## 1. ¿Qué son las plantillas en GitLab CI/CD? 📄

Una **plantilla** es simplemente un archivo `.yml` que contiene configuraciones comunes de CI/CD, y que puedes **incluir** desde otros `.gitlab-ci.yml` usando la palabra clave `include`.

---

## 2. Tipos de inclusión `include:` 🔗

GitLab permite incluir plantillas desde:

| Tipo                  | Descripción                                                  |
|-----------------------|--------------------------------------------------------------|
| `local:`              | Archivos dentro del mismo repositorio                        |
| `project:`            | Archivos en otro repositorio del mismo GitLab               |
| `remote:`             | Archivos alojados en una URL pública                         |
| `template:`           | Plantillas predefinidas por GitLab (por ejemplo: Docker, Go) |

---

## 3. Ejemplo usando `include:local` 📁

Estructura del repo:

├── .gitlab-ci.yml

└── ci-templates/

└── common-jobs.yml

```bash

`.gitlab-ci.yml`:

```yaml
include:
  - local: 'ci-templates/common-jobs.yml'

stages:
  - test
  - deploy
```

ci-templates/common-jobs.yml:
```yaml
run_tests:
  stage: test
  script:
    - echo "Ejecutando tests..."

deploy_app:
  stage: deploy
  script:
    - echo "Desplegando aplicación..."
```
---

## 4. Ejemplo usando include:project 🚀

```yaml
include:
  - project: 'grupo/otro-repo'
    file: '/ci/jobs.yml'
    ref: 'main'
```
📌 Esto incluye un archivo desde otro repositorio dentro del mismo GitLab. Muy útil para compartir CI corporativa.

---

## 5. Ejemplo usando include:remote 🌐

```yaml
include:
  - remote: 'https://mi-servidor.com/ci-template.yml'
```
☁️ Ideal para plantillas externas comunes (aseguarnos de confiar en el origen).

---

## 6. Usar plantillas predefinidas include:template 📦

```yaml
include:
  - template: 'Jobs/SAST.gitlab-ci.yml'
```
🔒 GitLab ofrece plantillas oficiales para cosas como análisis de seguridad, linting o despliegue.

---

## 7. Reutilización con extends 🧬
Podemos crear bloques reutilizables y extenderlos en varios jobs:
```yaml
.default_job_template:
  script:
    - echo "Común a todos los jobs"

job1:
  extends: .default_job_template
  stage: test

job2:
  extends: .default_job_template
  stage: deploy
```
🚀 Con extends, podemos aplicar una configuración base a muchos jobs fácilmente.

---

## 9. Ejemplo completo 🧪

ci-templates/build-template.yml

```yaml
.build_template:
  script:
    - echo "Compilando aplicación..."
```

.gitlab-ci.yml

```yaml
include:
  - local: 'ci-templates/build-template.yml'

stages:
  - build

build_job:
  extends: .build_template
  stage: build
```
🎯 Resultado: el build_job usará el script definido en .build_template.



