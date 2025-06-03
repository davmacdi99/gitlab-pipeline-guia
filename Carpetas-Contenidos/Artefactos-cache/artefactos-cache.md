# Artefactos y Caché en GitLab CI/CD 📦⚡

En GitLab CI/CD, los **artefactos** y la **caché** son herramientas clave para compartir datos entre jobs y acelerar la ejecución de pipelines.

---

## 1. ¿Qué es un artefacto? 🎁

Un **artefacto** es un archivo o conjunto de archivos que un job genera y que quieres compartir con otros jobs o descargar después del pipeline.

Por ejemplo: resultados de compilación, informes de tests, archivos empaquetados.

---

## 2. ¿Qué es la caché? 🗃️

La **caché** guarda archivos que se reutilizan entre ejecuciones del pipeline para ahorrar tiempo y recursos.

Por ejemplo: dependencias descargadas (npm, pip), paquetes compilados, archivos temporales.

---

## 3. Diferencias clave 🔑

| Característica      | Artefactos                          | Caché                              |
|---------------------|-----------------------------------|-----------------------------------|
| Propósito           | Compartir resultados entre jobs   | Reutilizar archivos entre pipelines|
| Disponibilidad      | Solo durante el pipeline actual   | Disponible entre diferentes pipelines |
| Ubicación           | Descargable desde interfaz GitLab | Almacenada en runner/cache         |
| Ejemplo típico      | Binarios, informes, paquetes      | Librerías, dependencias            |

---

## 4. Cómo definir artefactos 🎯

```yaml
build_job:
  stage: build
  script:
    - make build
  artifacts:
    paths:
      - build/output/
    expire_in: 1 week
```
- paths: carpetas o archivos a guardar.

- expire_in: tiempo que estarán disponibles (opcional).

---

## 5. Cómo definir caché ⚡
```yaml
cache_job:
  stage: build
  script:
    - npm install
  cache:
    paths:
      - node_modules/
    key: ${CI_COMMIT_REF_SLUG}
```
- paths: carpetas a cachear.

- key: clave para identificar la caché (por defecto usa la rama).

---
  
## 6. Ejemplo combinado 🧩

```yaml
stages:
  - build
  - test

build_job:
  stage: build
  script:
    - npm install
    - npm run build
  cache:
    paths:
      - node_modules/
  artifacts:
    paths:
      - dist/
    expire_in: 2 days

test_job:
  stage: test
  script:
    - npm test
  cache:
    paths:
      - node_modules/
  dependencies:
    - build_job
  artifacts:
    when: always
    paths:
      - test-reports/
```
- El job build_job instala dependencias y las guarda en caché para acelerar futuras ejecuciones.

- Además genera un directorio dist/ que es guardado como artefacto.

- El job test_job reutiliza la caché de node_modules/, depende de build_job para acceder a sus artefactos, y guarda informes de test.

---
## 🧩 Ejemplo completo con caché y artefactos

```yaml
stages:
  - install
  - build
  - test
  - deploy
```
```yaml
install_dependencies:
  stage: install
  script:
    - cd frontend && npm install
    - cd ../backend && pip install -r requirements.txt
  cache:
    key: ${CI_COMMIT_REF_SLUG}
    paths:
      - frontend/node_modules/
      - backend/.venv/
```
### 🔍 ¿Qué hace?

- Instala las dependencias del frontend (npm) y del backend (pip).

- Usa una caché compartida por rama (CI_COMMIT_REF_SLUG) para:

  - node_modules/ → dependencias de npm.

  - .venv/ → entorno virtual de Python.

- Esto acelera el pipeline en ejecuciones futuras.

```yaml
build_frontend:
  stage: build
  script:
    - cd frontend && npm run build
  artifacts:
    paths:
      - frontend/dist/
    expire_in: 1 week
  dependencies:
    - install_dependencies
  cache:
    paths:
      - frontend/node_modules/
```
### 🔍 ¿Qué hace?

- Construye el frontend con Webpack/Vite/etc.

- Guarda la carpeta dist/ como artefacto para etapas siguientes (como test o deploy).

- Reutiliza el caché de node_modules/ (aunque también esté en el job anterior, lo vuelve a declarar aquí para asegurarse de usarlo).

- Usa dependencies para obtener artefactos si los hubiera desde install_dependencies.
  
```yaml
test_backend:
  stage: test
  script:
    - cd backend && pytest --junitxml=test-reports/report.xml
  artifacts:
    when: always
    paths:
      - backend/test-reports/
    expire_in: 3 days
  cache:
    paths:
      - backend/.venv/
  dependencies:
    - install_dependencies
```
### 🔍 ¿Qué hace?

- Ejecuta tests en el backend con pytest y genera un informe XML.

- Guarda los reportes de test como artefactos visibles en la interfaz de GitLab.

- Reutiliza la caché del entorno virtual Python (.venv/).

- Se asegura de tener las dependencias del job anterior (install_dependencies).
  
```yaml
deploy_job:
  stage: deploy
  script:
    - echo "Desplegando la app..."
  dependencies:
    - build_frontend
    - test_backend
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
      when: manual
```
### 🔍 ¿Qué hace?
- Job de despliegue manual, solo disponible en la rama main.

- Accede a los artefactos generados por los jobs anteriores (build_frontend, test_backend).

- No necesita usar caché (el despliegue final solo necesita artefactos generados).


## 🧠 Consejos para proyectos grandes

✅ Separar responsabilidades por etapas: instalación, build, tests y despliegue.

⚡ Cachear todo lo que no cambie frecuentemente (dependencias, compilaciones pesadas).

📦 Guardar como artefactos los outputs que se usarán en otros jobs o querremos descargar.

🔁 Usar dependencies cuando un job necesita artefactos de otro.

🧹 Limpiar artefactos automáticamente con expire_in para ahorrar espacio.

🛠️ Validar el .gitlab-ci.yml en GitLab para evitar errores de sintaxis.
