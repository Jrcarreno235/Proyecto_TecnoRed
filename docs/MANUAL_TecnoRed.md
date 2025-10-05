# Proyecto: TecnoRed Soluciones - Paquete entregable

## Contenido del paquete

- index.html
- Servicios.html
- servicio.html
- Gestion_servicios.html
- inicio_sesion.html
- css/estilos.css
- js/funciones.js
- img/logo.png
- img/Slaider.png

## Resumen del proyecto

Este proyecto es una **aplicación web estática** (HTML/CSS/JS) que simula el sitio corporativo y una pequeña gestión de servicios (CRUD en memoria) para _TecnoRed Soluciones_. No requiere servidor: se ejecuta abriendo `index.html` en un navegador moderno.

### Tipo de "base de datos"

- **Actual**: No hay base de datos real. Los servicios se mantienen en memoria en la variable JavaScript `services` (array de objetos) dentro de `Gestion_servicios.html`.
- **Formato**: objetos JavaScript en memoria. Las imágenes subidas se convierten a **Data URLs (base64)** y se asignan a `service.image`.
- **Persistencia**: Actualmente **NO** persiste entre recargas del navegador. Opciones de mejora:
  - Guardar/recuperar `services` en `localStorage` (cliente).
  - Exponer una API REST (Node/Express, Python Flask, etc.) y almacenar en JSON o base de datos (SQLite/MySQL/Postgres).
  - Servir y leer un archivo plano `services.json` desde un servidor.

## Vistas / Páginas y su función

1. **index.html** — Página principal / Home. Describe la empresa, presenta servicios y CTA.
2. **Servicios.html** — Lista de tarjetas de servicios (visual). Permite seleccionar un servicio y enviar mensaje por WhatsApp.
3. **servicio.html** — Página de detalle de un servicio (ejemplo: Seguridad informática) con botón de contratación que redirige a Gestión de servicios.
4. **Gestion_servicios.html** — Interfaz administrativa para **gestionar servicios**: lista, agregar, editar, eliminar. Contiene el arreglo `services` inicial.
5. **inicio_sesion.html** — Formulario simulado de inicio de sesión (validación en cliente).
6. **css/estilos.css** — Estilos compartidos.
7. **js/funciones.js** — JavaScript auxiliar (actualmente con un `console.log`).

## Flujo de uso (cómo funciona)

- Usuario abre `index.html` → navega, ve servicios y puede pedir asesoría (abre WhatsApp).
- En `Servicios.html` el usuario puede pulsar una tarjeta para solicitar info vía WhatsApp (mensaje preformateado).
- `servicio.html` muestra detalles y al presionar "Contratar ahora" redirige a `Gestion_servicios.html`.
- En `Gestion_servicios.html` se puede:
  - Ver la lista de servicios (renderizada por `renderServices()`).
  - Agregar un servicio (formulario): puede elegir icono Font Awesome o subir imagen (se convierte a Data URL vía FileReader).
  - Editar servicio: carga datos en formulario para editar.
  - Eliminar servicio.
  - Validaciones cliente para campos requeridos.
- Todo se ejecuta completamente en el cliente (JavaScript). No hay llamadas a servidor.

## Descripción de las funciones / características

A continuación se listan y describen funciones / "features" implementadas en los archivos del proyecto:

1. **solicitarAsesoria()** — Abre WhatsApp con mensaje predefinido. (index, servicios, servicio, gestión).
2. **renderServices()** — Renderiza la tabla HTML con los objetos `services` en `Gestion_servicios.html`.
3. **toggleImageType()** — Cambia entre campo icono (texto) e imagen (file input) en el formulario.
4. **previewImage(event)** — Lee la imagen seleccionada y la convierte a Data URL (FileReader) para vista previa y almacenamiento temporal.
5. **toggleDiscount()** — Muestra/oculta el campo % descuento si la promoción está activa.
6. **showAddForm()** — Limpia y prepara el formulario para agregar un nuevo servicio.
7. **editService(index)** — Carga los datos del servicio seleccionado en el formulario para editar.
8. **deleteService(index)** — Elimina servicio del arreglo `services` tras confirmación.
9. **Formulario submit handler** — Captura `submit`, valida campos, construye objeto `service` y lo agrega o actualiza en el arreglo `services`.
10. **currentImageData** — Variable que mantiene la Data URL de la imagen cargada mientras se edita/crea.
11. **imagePreview** — Elemento `<img>` para mostrar vista previa de imagen subida.
12. **services (array)** — Arreglo inicial con objetos de ejemplo (Soporte, Redes, Desarrollo, etc.).
13. **selectService(service)** — En `Servicios.html`, prepara mensaje y abre WhatsApp con info del servicio seleccionado.
14. **navigateTo(section)** — Función demo para navegación entre secciones (Servicios.html).
15. **contactar()** — En `servicio.html` muestra alerta y redirige a `Gestion_servicios.html`.
16. **Login validation** — Validaciones simples en `inicio_sesion.html` que muestran errores y simulan inicio de sesión.
17. **Smooth scrolling** — Código que anima el desplazamiento para enlaces `href="#"` con target en la misma página.
18. **IntersectionObserver effects** — Efecto de aparición al hacer scroll para `.service-card` y `.service-icon`.
19. **Card hover effects** — JavaScript y CSS para efecto visual al entrar/salir del hover en tarjetas.
20. **Image size check** — `previewImage` valida tamaño máximo (5MB) y rechaza imágenes mayores.

## Instrucciones para ejecutar localmente (rápido)

1. Descargar y descomprimir el paquete, o abrir los archivos locales.
2. Abrir `index.html` en un navegador moderno (Chrome, Edge, Firefox).
   - Recomendado: usar un servidor local si hay problemas con FileReader o rutas (por ejemplo `python -m http.server 8000` en la carpeta del proyecto).
3. Navegar a `Gestion_servicios.html` para probar la gestión (agregar/editar/eliminar).
4. Para probar subida de imagen: en el formulario seleccionar "Imagen", elegir archivo <5MB — se mostrará la vista previa.
5. Para ver efectos y enlaces a WhatsApp, se requiere conexión a internet para usar la URL de `wa.me` y para cargar Font Awesome desde CDN (si falta, las fuentes de íconos pueden no mostrarse).

## Recomendaciones de mejora (persistencia y despliegue)

- **Persistencia cliente**: Guardar `services` en `localStorage` para mantener cambios tras recarga:
  - `localStorage.setItem('services', JSON.stringify(services));`
  - Al cargar: `const saved = localStorage.getItem('services'); if (saved) services = JSON.parse(saved);`
- **Servidor simple (opcional)**: Añadir un endpoint REST (Node/Express) que sirva `GET/POST/PUT/DELETE` y almacene en `services.json`.
- **Autenticación**: Implementar backend y login seguro (hash de contraseñas).
- **Validaciones**: Validaciones adicionales (precio >=0, cantidad >=0, nombres únicos).
- **Optimización de imágenes**: guardar versiones redimensionadas en servidor.

## Estructura sugerida para pasar a producción (ejemplo)

- `/api` -> endpoints REST
- `/data/services.json` -> archivo plano con servicios
- `/public` -> archivos estáticos (los presentes)
- Base de datos: SQLite para prototipo, PostgreSQL para producción.

## Notas finales

- Este paquete es una entrega de la aplicación tal como fue desarrollada: **front-end estático con lógica en JavaScript**.
- Si desea que prepare una versión con persistencia (JSON + servidor ligero) o un instalador (`docker-compose`, servidor Node) puedo generarla y adjuntarla también.

El sistema guarda automáticamente los servicios creados, editados o eliminados
en el almacenamiento local del navegador (`localStorage`).

- **Al cargar la página:** se leen los datos guardados si existen.
- **Cada vez que se agrega, edita o elimina un servicio:** se actualiza el `localStorage` con el nuevo arreglo `services`.
- Los datos persisten aunque se cierre el navegador, mientras no se borre la caché local.

Para reiniciar los datos, abra la consola del navegador (F12 → Console) y ejecute:

```js
localStorage.removeItem("services");
```

Esto restablecerá los servicios por defecto al volver a cargar la página.
