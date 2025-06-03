# ✅ Buenas Prácticas en GitLab CI/CD

Un pipeline bien diseñado no solo automatiza tareas, también mejora la calidad del código, reduce errores y acelera el ciclo de desarrollo. Aquí se muestra un conjunto de buenas prácticas que nos ayudarán a crear pipelines más seguros, mantenibles y eficientes. 🚀

---

## 1. Organizar los `stages` por lógica 🧱

Definir las etapas (`stages`) de forma que reflejen el flujo natural del proyecto:

```yaml
stages:
  - build
  - test
  - deploy
```
🔸 Podemos añadir otras como lint, package, release, cleanup si el proyecto lo necesita.
🔸 Evita tener demasiadas stages que compliquen el flujo innecesariamente.

---

## 2. Usar variables para evitar repeticiones ♻️

Evitar valores duplicados y facilitar el mantenimiento:

```yaml
variables:
  DEPLOY_DIR: "/home/app"

deploy_job:
  script:
    - echo "Desplegando en $DEPLOY_DIR"
```

🔐 Definir variables sensibles desde la interfaz web de GitLab, no en el código.

---

## 3. Usar rules para controlar cuándo se ejecutan los jobs 🧠

Evitar que todos los jobs se ejecuten siempre. Usar condiciones como ramas o tipos de evento:

```yaml
rules:
  - if: '$CI_COMMIT_BRANCH == "main"'
    when: manual
```
✅ Esto ahorra tiempo y recursos, y nos da control sobre los entornos.

---

## 4. Aprovechar artifacts y cache 📦

- artifacts: Guarda archivos generados para otros jobs o para descargar desde GitLab.

- cache: Mejora el rendimiento entre ejecuciones.

```yaml
artifacts:
  paths:
    - build/

cache:
  key: build-cache
  paths:
    - node_modules/
```

📌 Usar cache para dependencias, y artifacts para resultados del proceso.

---

## 5. Usar entornos (environment) para trazabilidad 🌍

Definir environment para que GitLab registre dónde se despliega la app:

```yaml
environment:
  name: production
  url: https://miapp.com
```

Esto nos permite ver desde la interfaz:

- Qué commit está en qué entorno

- Historial de despliegues

- Acceso directo al entorno

---

## 6. Empezar simple, luego escalar 🚶‍♂️➡️🏃‍♀️

Comenzar con un pipeline sencillo y posteiormente ir añadiendo complejidad poco a poco:

✅ Primero: build → test → deploy
🧪 Luego: variables, rules, artifacts, entornos dinámicos…

---

## 7. Valida tu .gitlab-ci.yml 🧪

Antes de hacer commit, usar el validador oficial de GitLab:

🔗 CI/CD > Editor de pipeline o CI Lint

Nos ayudará a:

- Detectar errores de sintaxis

- Previsualizar el pipeline

- Comprobar reglas y ramas

---

## 8. Documentar y comentar 📝

```yaml
# Este job solo se ejecuta en producción y requiere ejecución manual
deploy_prod:
  stage: deploy
  when: manual
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```
📌 Un pipeline bien documentado es más fácil de mantener y escalar.

---

## 9. Usar include: para mantener los pipelines limpios 📂

Dividir el .gitlab-ci.yml en archivos reutilizables por entorno o por tipo de tarea:

```yaml
include:
  - local: 'templates/deploy.yml'
  - project: 'grupo/templates'
    file: '/common.yml'
    ref: 'main'
```
🔁 Favorece la reutilización y reduce duplicaciones.

---

## 10. Evita ejecutar pipelines innecesarios 🔁

Podemos limitar cuándo se ejecuta un pipeline completo, por ejemplo:

```yaml
workflow:
  rules:
    - if: '$CI_COMMIT_BRANCH == "main"'
```
🔒 Útil para pipelines pesados o que solo deben correr en producción.

---

### 🏁 Conclusión!
- 🔐 Cuidar la seguridad de nuestros tokens y variables.

- ⚙️ Automatizar, pero con control.

- 🧠 Asegurarnos de entender qué hace cada job.

- 🫶 Compartir buenas prácticas con nuestro equipo.

  
