# Análisis Personal del Despliegue y Seguridad

## 1. ¿Qué vulnerabilidades previenen los encabezados implementados?

Los encabezados configurados en el archivo `staticwebapp.config.json` funcionan como una primera línea de defensa en el navegador del cliente, reduciendo significativamente la superficie de ataque de la aplicación web.

### XSS (Cross-Site Scripting)

El uso de **Content-Security-Policy (CSP)** permite controlar de manera estricta qué recursos pueden ejecutarse en la aplicación. Al definir un hash SHA-256 para scripts y una lista blanca de dominios confiables, se evita que scripts inyectados de forma maliciosa puedan ejecutarse, incluso si logran insertarse en el DOM. Esto es especialmente importante en aplicaciones modernas donde el contenido dinámico es frecuente.

### Clickjacking

El encabezado:

```http
X-Frame-Options: DENY
```

impide que la aplicación sea cargada dentro de un iframe en sitios externos. Esto previene ataques donde un usuario es engañado para interactuar con elementos invisibles superpuestos, lo cual podría derivar en acciones no intencionadas como clics en botones sensibles.

### MIME-Sniffing

El encabezado:

```http
X-Content-Type-Options: nosniff
```

obliga al navegador a respetar el tipo de contenido definido por el servidor. Sin esta protección, algunos navegadores podrían interpretar incorrectamente archivos (por ejemplo, tratar un archivo como script ejecutable), lo que abriría la puerta a ataques basados en la manipulación de contenido.

### Intercepción de Datos

La implementación de **HTTP Strict Transport Security (HSTS)** garantiza que todas las comunicaciones entre el cliente y el servidor se realicen mediante HTTPS. Esto evita ataques de tipo “man-in-the-middle” y ataques de degradación de protocolo, en los que un atacante intenta forzar el uso de HTTP para interceptar información sensible.

---

## 2. ¿Qué aprendiste sobre la relación entre despliegue y seguridad web?

Durante el proceso se evidenció que la seguridad no es una etapa posterior al desarrollo, sino un componente transversal que debe integrarse desde el inicio, en línea con las prácticas de DevSecOps.

Un despliegue en la nube, como en Azure Static Web Apps, no solo implica publicar archivos, sino también definir explícitamente reglas de seguridad que controlen cómo se comporta la aplicación en el entorno real. Estas configuraciones afectan directamente tanto la protección como la funcionalidad del sistema.

Además, se comprendió que existe una relación directa entre seguridad y disponibilidad. Una política demasiado restrictiva puede bloquear recursos legítimos como fuentes, imágenes o scripts externos, afectando la experiencia del usuario. Por el contrario, una política demasiado permisiva puede exponer la aplicación a vulnerabilidades. Por ello, es necesario encontrar un equilibrio adecuado mediante pruebas y ajustes iterativos.

---

## 3. ¿Qué desafíos encontraste en el proceso?

### Configuración del CSP

El principal desafío fue lograr un equilibrio entre seguridad y funcionalidad. Angular, al ser un framework que maneja contenido dinámico y múltiples recursos externos, requiere permisos específicos en la política CSP. Fue necesario identificar con precisión qué dominios y tipos de recursos debían ser permitidos, especialmente para fuentes externas como Google Fonts y servicios como PokeAPI, sin comprometer la seguridad general.

### Gestión de Rutas

Otro reto importante fue asegurar que el archivo `staticwebapp.config.json` se incluyera correctamente en el proceso de compilación. Esto implicó modificar el archivo `angular.json` para garantizar que la configuración de seguridad estuviera presente en el entorno de producción, ya que de lo contrario los encabezados no se aplicarían.

### Depuración en Tiempo Real

La interpretación de los errores generados por violaciones de CSP en la consola del navegador fue un proceso clave de aprendizaje. Estos mensajes, aunque técnicos, proporcionan información precisa sobre qué recursos están siendo bloqueados. Aprender a analizarlos permitió ajustar la configuración de manera controlada, evitando soluciones generales que pudieran debilitar la seguridad.

---

## Conclusión

El proceso permitió comprender que la seguridad web es un aspecto fundamental del despliegue y no un complemento opcional. Implementar encabezados de seguridad adecuados fortalece la protección de la aplicación frente a amenazas comunes, pero también exige un entendimiento profundo del comportamiento de la aplicación.

En este contexto, el desarrollador no solo construye funcionalidades, sino que también define las reglas bajo las cuales la aplicación puede operar de forma segura. Esto implica un enfoque continuo de prueba, ajuste y validación para garantizar tanto la protección como el correcto funcionamiento del sistema.
