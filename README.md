# Guía de Pipelines en GitLab CI/CD

🚀 Una guía completa y práctica para entender, crear y optimizar pipelines en GitLab.

> ✍️ **Hola! Soy David**.  
> Este repositorio nace de mi interés por aprender y profundizar en GitLab CI/CD. Mi objetivo es compartir lo que voy >aprendiendo con explicaciones claras, ejemplos prácticos y buenas prácticas, tanto para quienes están empezando como >para quienes ya trabajan en el sector.

---

## 📘 Descripción

Esta guía está pensada para desarrolladores, DevOps y equipos que quieran incorporar automatización en sus flujos de trabajo mediante **GitLab CI/CD**. Aquí encontrarás ejemplos claros, explicaciones detalladas y buenas prácticas para sacar el máximo provecho de los pipelines.

...

## 📂 Contenido

- [Introducción a GitLab CI/CD](/Carpetas-Contenidos/Introduccion/introduccion.md)
- [Estructura de un `.gitlab-ci.yml`](/Carpetas-Contenidos/Estructura/estructura-yml.md)
- [Uso de Variables](/Carpetas-Contenidos/Variables/variables.md)
- [Condiciones y Reglas](/Carpetas-Contenidos/Condiciones-Reglas/condiciones-reglas.md)
- [Artefactos y Caché](/Carpetas-Contenidos/Artefactos-cache/artefactos-cache.md)
- [Plantillas y Reutilización](/Carpetas-Contenidos/Plantillas/plantillas-reutilizacion.md)
- [Despliegues Manuales y Automáticos](/Carpetas-Contenidos/Despliegues/despliegues.md)
- [Environments en GitLab](/Carpetas-Contenidos/Enviroments/environments.md)
- [Buenas Prácticas](/Carpetas-Contenidos/BuenasPracticas/buenas-practicas.md)

---

## 📄 Ejemplo básico

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
---

## 🙌 Contribuciones
> ¿Tienes sugerencias, mejoras o quieres compartir un caso de uso real?
> ¡Puedes abrir un issue o un pull request! Todas las contribuciones son bienvenidas.
> 
---

## 💡 ¿Por qué este proyecto?
> Además de reforzar mis conocimientos, este repositorio representa mi forma de aprender: compartiendo.
> Espero que te resulte útil, y si estás contratando perfiles DevOps junior con muchas ganas de crecer, ¡me encantaría 
> hablar contigo!

## 🔗 Contacto

[![LinkedIn](https://img.shields.io/badge/LinkedIn-David%20Macias-blue?logo=linkedin&style=flat-square)](https://www.linkedin.com/in/davidmaciasdiaz/)  
✉️ davmacdi99@gmail.com







