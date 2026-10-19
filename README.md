# Control de Roll de Turnos

Aplicación web para llevar el control del roll de turnos de personal **Sindicalizado** y **Empleado**: Área, Cuadrilla, Ficha, Nombre, Tipo de empleado, Nota y los 7 días de la semana (nombre + fecha dd/mm/aaaa).

## ⚠️ Importante sobre SharePoint
Si subes esta app usando la opción **"+ New → HTML page"** de Site Pages, SharePoint la ejecuta dentro de un iframe en modo "sandbox" por seguridad, lo cual **bloquea el guardado permanente** (localStorage) y la instalación como app (service worker). La app detecta esto y muestra un aviso, pero no es la forma recomendada de usarla.

### Forma recomendada
1. Sube la **carpeta completa** (sin comprimir) a una biblioteca normal de documentos en SharePoint (ej. "Documents"), usando **Upload → Folder** o arrastrando la carpeta.
2. Cada usuario descarga esa carpeta a su celular, tablet o PC, y abre `index.html` directamente desde el explorador de archivos (doble clic).
3. Así la app guarda datos de forma permanente, se puede instalar como aplicación (PWA) y funciona sin Internet.

## Contenido del paquete
- `index.html` – Aplicación completa (interfaz + lógica + datos iniciales incrustados).
- `manifest.json` – Metadatos para instalación como app (PWA).
- `service-worker.js` – Habilita el funcionamiento sin conexión a Internet (requiere HTTPS o archivo local).
- `seed.json` – Copia de referencia de los datos iniciales (no se usa en tiempo de ejecución).
- `icons/` – Íconos de la aplicación.

## Funcionalidades
- Alta, edición y eliminación de empleados (Área, Cuadrilla, Ficha, Nombre, Tipo, Nota).
- Captura de turno por día con selector de códigos (1, 2, 3, E2, D, F, V, G) + código personalizado.
- Edición de la leyenda de códigos (agregar/quitar/cambiar color y descripción).
- Filtro por tipo de empleado, área, cuadrilla y búsqueda por nombre/ficha.
- Navegación por semana (anterior/siguiente/hoy).
- **Generar 3 semanas automáticamente**: toma la semana visible en pantalla como plantilla y proyecta las siguientes 3 semanas, rotando los turnos según la secuencia configurable (por defecto 3 → 2 → 1 → 3...) y manteniendo los descansos (y cualquier otro código) en el mismo día de la semana, salvo que ya exista un turno capturado manualmente en la semana destino (puedes elegir sobrescribir o respetar lo ya capturado).
- **Historial de cambios**: registra automáticamente cada turno modificado, alta/edición/baja de empleados, cambios en la leyenda, generaciones automáticas, respaldos y archivos vinculados, con fecha/hora. Se puede buscar, borrar o exportar a Excel.
- **Guardado automático en un archivo del equipo** (computadora con Chrome/Edge): vincula un archivo local (puede estar dentro de tu carpeta OneDrive o de una biblioteca de SharePoint sincronizada) y cada cambio se escribe ahí automáticamente, además de guardarse en el navegador. Si el archivo vive en una carpeta sincronizada, también queda respaldado en SharePoint/OneDrive en la nube sin pasos adicionales. Si el navegador pierde el permiso (por ejemplo al reabrir), la app lo indica y permite reconectar con un clic.
- **Respaldo programado y snapshots automáticos**:
  - Define una frecuencia (diario, semanal, quincenal o mensual) para recibir un recordatorio visible en la parte superior de la app cuando sea momento de descargar un respaldo (.json) manualmente.
  - Activa los **snapshots automáticos internos**: cada vez que abras la app (respetando la frecuencia elegida), se guarda una copia completa de los datos dentro del propio dispositivo, conservando las últimas 10 copias, restaurable o descargable individualmente.
- Exportar a Excel (.xlsx) y a PDF la vista filtrada de la semana en turno.
- Instalable como PWA en celular, tablet y computadora; funciona offline (cuando se abre directamente, no dentro de un visor sandboxed).

## Persistencia de datos por tipo de dispositivo
| Dispositivo | Guardado automático en el navegador | Guardado automático en archivo (OneDrive/SharePoint) |
|---|---|---|
| Computadora (Chrome/Edge) | Sí | Sí, vinculando un archivo desde **💾 Respaldo** |
| Celular (Android/iPhone) | Sí | No disponible (limitación del navegador móvil); usa respaldo manual/snapshots |
| Tablet | Sí | No disponible; usa respaldo manual/snapshots |
| Página HTML sandboxed de SharePoint ("+ New → HTML page") | No (restricción de seguridad de SharePoint) | No disponible |

## Recomendación de uso
En computadora, vincula un archivo dentro de tu carpeta de OneDrive/SharePoint sincronizada la primera vez que uses la app (botón **💾 Respaldo → Vincular archivo**). En celular o tablet, configura la frecuencia de respaldo y activa los snapshots automáticos, y descarga un respaldo manual de vez en cuando para tener una copia fuera del dispositivo.
