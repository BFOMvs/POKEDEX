Proceso de Despliegue - PokeDex Angular
Este documento detalla los pasos técnicos realizados para el despliegue de la aplicación en Azure Static Web Apps y las configuraciones necesarias para garantizar su correcto funcionamiento.

1. Integración Continua (CI/CD)
El despliegue se gestionó de manera automatizada mediante GitHub Actions. Cada vez que se realiza un push a la rama main, Azure dispara un flujo de trabajo (workflow) que compila la aplicación Angular y la despliega en la infraestructura de la nube.

2. Configuración de Entorno de Producción
Se ajustó el archivo src/environments/environment.prod.ts para asegurar que las peticiones a la API y las imágenes se carguen desde las fuentes correctas en el entorno de producción, evitando errores de rutas locales (localhost).

3. Manejo de Rutas (Navigation Fallback)
Para evitar errores 404 al recargar la página o navegar directamente a una ruta (ej. /pokemon/1), se configuró una regla de "fallback" en el archivo de configuración de Azure:

Acción: Todas las rutas de navegación que no correspondan a archivos físicos (imágenes, js, css) se redirigen al index.html, permitiendo que Angular maneje el enrutamiento interno.

4. Inclusión de Archivos de Configuración en el Build
Para que Azure reconozca las reglas de seguridad y navegación, se modificó el archivo angular.json:

Se añadió "src/staticwebapp.config.json" dentro del array de assets.

Esto garantiza que, al ejecutar el comando ng build, el archivo de configuración se incluya en la carpeta final de distribución (dist/) que Azure lee.

5. Verificación de Despliegue
URL del Sitio: https://brave-sand-09306fa0f.7.azurestaticapps.net

Estado de Seguridad: Calificación A verificada en Security Headers de Snyk
