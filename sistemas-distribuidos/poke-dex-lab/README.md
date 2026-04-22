Análisis Personal del Despliegue y Seguridad
1. ¿Qué vulnerabilidades previenen los encabezados implementados?
Los encabezados configurados en el archivo staticwebapp.config.json actúan como una capa de defensa proactiva:

XSS (Cross-Site Scripting): Prevenido mediante el Content-Security-Policy (CSP) al usar un Hash SHA-256 para scripts y una lista blanca de dominios permitidos, evitando la ejecución de código malicioso inyectado.

Clickjacking: Mitigado con X-Frame-Options: DENY, que impide que el sitio sea cargado dentro de un iframe en dominios externos.

MIME-Sniffing: Evitado con X-Content-Type-Options: nosniff, obligando al navegador a respetar el tipo de contenido declarado por el servidor.

Intercepción de Datos: El encabezado HSTS asegura que todas las comunicaciones se realicen exclusivamente a través de HTTPS, protegiendo contra ataques de degradación de protocolo.

2. ¿Qué aprendiste sobre la relación entre despliegue y seguridad web?
He aprendido que la seguridad no es un proceso aislado, sino una parte integral del ciclo de vida del despliegue (DevSecOps):

Un despliegue exitoso en la nube (como Azure Static Web Apps) requiere que el desarrollador defina explícitamente las políticas de confianza.

La seguridad web influye directamente en la disponibilidad de los recursos; una política mal configurada puede bloquear elementos legítimos como imágenes o fuentes.

3. ¿Qué desafíos encontraste en el proceso?
Configuración del CSP: El mayor reto fue equilibrar la seguridad estricta para obtener la calificación A+ con la funcionalidad de Angular, especialmente al manejar las fuentes de Google y las imágenes externas de la PokeAPI.

Gestión de Rutas: Asegurar que el archivo de configuración fuera incluido correctamente en el paquete de producción mediante la edición del angular.json.

Depuración en Tiempo Real: Aprender a interpretar los errores de violación de política en la consola del navegador para ajustar los permisos quirúrgicamente.