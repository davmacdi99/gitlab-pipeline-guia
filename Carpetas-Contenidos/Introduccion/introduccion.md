# 🧭 Introducción a GitLab CI/CD

Imagina que cada vez que haces cambios en tu código (por ejemplo, agregas una nueva funcionalidad), alguien se encargara de revisar que todo siga funcionando, instalar dependencias, hacer pruebas y, si todo está bien, preparar tu aplicación para que se publique.

Eso es exactamente lo que hace **GitLab CI/CD** por nosotros.

## ¿Qué significa CI/CD?

- **CI** (Integración Continua): Es una forma automática de revisar y probar el código cada vez que hacemos un cambio en el proyecto.
- **CD** (Entrega o Despliegue Continuo): Es el proceso de llevar ese código probado y listo a un servidor para que otros puedan usarlo (como subir una app a producción).

## ¿Por qué usarlo?

GitLab CI/CD nos ayuda a:

✅ Ahorrar tiempo automatizando tareas repetitivas  
✅ Evitar errores al comprobar el código cada vez que hacemos un cambio  
✅ Desplegar la aplicación sin tener que hacerlo manualmente  
✅ Trabajar en equipo sin pisarse el trabajo

Todo esto sucede automáticamente cada vez que hacemos un **push** (subes cambios) al repositorio en GitLab del que estemos trabajando. 

## ¿Cómo funciona?

Solo necesitamos un archivo llamado **`.gitlab-ci.yml`** en el repositorio.  
En ese archivo escribimos paso a paso qué quieres que GitLab haga por nosotros, por ejemplo:

- Instalar dependencias
- Correr pruebas
- Compilar tu aplicación
- Subirla a un servidor

GitLab lee ese archivo y se encarga de ejecutar todo, en el orden que ya definimos.

---

> Esta guía está pensadda para aprender desde lo más básico hasta ejemplos prácticos para crear nuestros propios pipelines y automatizar tareas paso a paso.
> Toda ayuda aportada será bien recibida! Dado que este repositorio nace de las ganas de aprender sobre esta gran herramienta!

¡Vamos allá!
