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


