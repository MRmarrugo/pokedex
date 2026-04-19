Proceso de Despliegue y Configuración Técnica: PokeDex
Este documento detalla la metodología técnica aplicada para la puesta en producción de la aplicación PokeDex en la infraestructura de Microsoft Azure, garantizando la estabilidad y la implementación de políticas de seguridad para Pueblo Paleta Inc.

1. Preparación y Compilación del Proyecto
La aplicación, desarrollada en Angular, requirió un proceso de optimización antes de ser enviada a la nube.

Gestión de Dependencias: Al trabajar con el repositorio, se detectó que el script de pre-instalación fallaba por la ausencia de un entorno Git inicializado.

Solución: Se ejecutó git init seguido de npm install para habilitar correctamente los hooks de desarrollo.
Generación del Build: Se utilizó el comando:

Bash

npm run build

Esto generó la carpeta /dist/pokedex-angular, conteniendo los archivos minificados (HTML, JS, CSS) listos para producción.

2. Despliegue en la Nube (Azure App Service)
Se seleccionó Azure App Service (Plan F1 Free) para hospedar la aplicación, siguiendo estos pasos técnicos:

Transferencia de Archivos: Se accedió al servidor mediante las herramientas avanzadas de Azure y se cargó el contenido del build directamente en el directorio raíz del servidor web (wwwroot).

Servidor de Aplicaciones: El despliegue se apoya en un servidor Microsoft-IIS/10.0, lo que permitió gestionar la configuración mediante un archivo de control de servidor.

Resolución de Rutas Estáticas: Se corrigió un error 404 relacionado con la ubicación de la carpeta assets. Se reestructuró el directorio en el servidor para que las rutas relativas de Angular coincidieran con la ubicación física de las imágenes.

3. Implementación de Seguridad (Calificación A)
Para cumplir con los requisitos de seguridad, se configuraron encabezados HTTP para proteger el sitio contra ataques comunes.

Cabeceras de Seguridad Configuradas:
Strict-Transport-Security (HSTS): Configurado con un max-age de un año (31536000) e incluyendo subdominios, para garantizar que la comunicación siempre sea bajo HTTPS.

X-Frame-Options (DENY): Configurado para evitar que la PokeDex sea embebida en otros sitios, eliminando el riesgo de Clickjacking.

X-Content-Type-Options (nosniff): Bloquea el “MIME-sniffing”, obligando al navegador a seguir estrictamente el tipo de contenido declarado.

Referrer-Policy (no-referrer): Asegura la privacidad del usuario al no enviar información de origen cuando se sale de la aplicación.

Permissions-Policy: Se desactivó el acceso a sensores innecesarios como acelerómetro, cámara y micrófono.

Content Security Policy (CSP) Personalizado:
Se implementó una política robusta que permite el funcionamiento de la app mientras restringe orígenes desconocidos:

Conexiones: Se autorizaron pokeapi.co y beta.pokeapi.co para la obtención de datos.

Imágenes: Se habilitó la carga desde orígenes externos de confianza como raw.githubusercontent.com y assets.pokemon.com.

Estilos y Scripts: Se permitió 'unsafe-inline' para no romper la inyección de estilos dinámica de Angular, manteniendo la seguridad de los dominios fuente.

4. Validación de Resultados
La implementación fue validada exitosamente mediante herramientas de auditoría externa:

URL: https://pokedex-smm-cgcggmfnasb0dggk.eastus-01.azurewebsites.net/

Servidor: IIS 10.0 con soporte para ASP.NET.

Estado de Seguridad: Calificación A obtenida en el escaneo de encabezados, cumpliendo con los objetivos de protección de datos y robustez de Pueblo Paleta Inc.
