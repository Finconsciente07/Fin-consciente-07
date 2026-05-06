# FINCO — Sistema de Gestión

Sistema de gestión interno para FINCO (Finanzas Conscientes). Maneja clientes, CRM, finanzas, servicios, resultados y calendario en una sola interfaz, con la estética verde profundo + dorado de la marca.

Es un prototipo funcional en un único archivo HTML. Todo lo que cargues por la interfaz (leads, clientes) se guarda en el navegador (localStorage) y persiste al recargar.

---

## Cómo subirlo a GitHub

1. Creá un repositorio nuevo en GitHub (puede ser privado).
2. Renombrá el archivo `finco-sistema.html` a `index.html` (esto es importante para GitHub Pages).
3. Subí los dos archivos al repositorio: `index.html` y `README.md`.
4. Si querés tenerlo accesible online, andá a **Settings → Pages**, en *Source* elegí *Deploy from a branch*, seleccioná `main` y `/root`. En 1-2 minutos vas a tener la URL pública.
5. Listo.

---

## Cómo usarlo

### Login

Al abrir, te recibe la pantalla de login. Cualquier botón te entra (es un mockup, no hay backend real). Los datos están pre-llenados para acceso rápido en demo.

### Navegación

El sidebar tiene siete secciones:

- **Dashboard** — vista general con KPIs, cashflow, reuniones del día, pipeline resumido.
- **Clientes** — cartera activa con tabla detallada, KPIs de la cartera y tabs de estado.
- **CRM** — pipeline kanban de leads y prospectos con KPIs de conversión.
- **Finanzas** — P&L, mix de ingresos, movimientos del mes, facturación.
- **Servicios** — proyectos activos P01 / P02 con progreso.
- **Resultados** — KPIs internos de FINCO + impacto Growth Partner por cliente.
- **Calendario** — vista mensual con todas las reuniones, color-codeadas por tipo.

### Cargar datos reales

Hay dos formas:

**1. Por la interfaz (recomendado)**

- En *CRM* → botón `+ Nuevo lead` abre un form completo. Completás y queda en el pipeline.
- En *Clientes* → botón `+ Alta de cliente` abre el form para sumar a la cartera.
- Las celdas de la tabla y las cards muestran un botón × al pasar el mouse para eliminar.

Todo lo que cargues se guarda automáticamente en `localStorage`. Si recargás la página o cerrás el navegador, tus datos siguen ahí.

**2. Por consola (avanzado)**

Abrí la consola del navegador (F12 en Chrome) y tenés acceso al objeto `FINCO`:

```javascript
// Ver datos actuales
FINCO.leads
FINCO.clients

// Agregar un lead programáticamente
FINCO.addLead({
  name: 'Juan Pérez',
  company: 'Pérez & Asoc',
  industry: 'Contabilidad',
  value: 1200,
  stageIdx: 0,        // 0=Leads, 1=Diagnóstico, 2=Propuesta, 3=Negociación
  source: 'Referido',
  email: 'juan@perez.com',
  notes: 'Equipo de 6 personas, todo en Excel'
});

// Agregar un cliente
FINCO.addClient({
  name: 'María Sosa',
  company: 'Sosa Consultora',
  industry: 'Consultoría',
  service: 'P02',     // 'P01' o 'P02'
  status: 'active',   // 'active' o 'onboarding'
  fee: 380,
  startDate: '2026-05-01',
  email: 'maria@sosa.com',
  notes: '...'
});

// Exportar todo a JSON (descarga archivo)
FINCO.exportJSON();

// Resetear: borra todo lo cargado y recarga la página
FINCO.reset();
```

---

## Estructura del archivo

Todo vive en `index.html`. Está organizado por bloques bien marcados con comentarios:

- **HTML**: login screen, sidebar, top bar, 7 páginas (`#page-dashboard`, `#page-clientes`, etc.), modales de forms, ficha de cliente.
- **CSS**: variables de color al inicio (`:root`), luego layout, sidebar, páginas, calendario, ficha cliente, login, formularios.
- **JS**: capa de datos `FINCO` con persistencia, navegación, tabs, cliente detalle, calendario, login, búsqueda, modales.

### Paleta de colores

Toda la paleta está en variables CSS al inicio del archivo. Cambiala desde ahí si querés ajustar la marca:

```css
--green-deep:  #071a0e;
--green-rich:  #153d22;
--gold:        #c9a84c;
--gold-light:  #e8c97a;
--cream:       #f5f0e8;
```

### Tipografía

- **Cormorant Garamond** (serif elegante) para títulos y números destacados.
- **DM Sans** (sans-serif moderna) para el cuerpo de texto.

Ambas se cargan desde Google Fonts.

---

## Cómo personalizar

### Cambiar nombre del usuario

Buscá en el HTML:

```html
<div class="user-name">Juan Cruz</div>
<div class="user-role">Founder · FINCO</div>
```

Y reemplazá con tus datos. Lo mismo en el saludo del dashboard:

```html
<h1 class="page-title">Buen día, <em>Juan Cruz</em>.</h1>
```

### Borrar datos demo de las tablas

Los datos demo (12 clientes, 14 leads en CRM, etc.) están hardcodeados en el HTML para que el sistema "se vea lleno" y elegante. Si querés arrancar con la cartera vacía:

1. En `<tbody>` dentro de `#page-clientes`, borrá las `<tr>` que no necesites.
2. En cada `.pipeline-col` dentro de `#page-crm`, vaciá los `.pipeline-body`.
3. Actualizá los contadores `.pipeline-head-count` y los KPIs.

### Cambiar logo del navbar

El logo es la letra "F" en un círculo dorado. Si querés poner una imagen:

```html
<div class="brand-mark">F</div>
<!-- reemplazar con: -->
<img src="logo.png" class="brand-mark" style="object-fit:cover;">
```

---

## Limitaciones actuales (prototipo)

- **Single-user**: no hay auth real, no hay backend. Todo en localStorage.
- **Ficha de cliente**: muestra los datos demo de Carolina Méndez para cualquier cliente que abras (pendiente refactor a data por cliente).
- **Calendario**: vista mensual funcional, sin drag & drop ni edición de eventos.
- **Pipeline**: las cards no se mueven entre columnas con drag & drop (pendiente).
- **Sin export PDF / Excel**: planeado para próxima iteración.
- **localStorage**: tus datos viven en este navegador. Si abrís el sistema en otro dispositivo, no los ves. Para multi-dispositivo necesitás un backend real.

---

## Próximos pasos sugeridos

Cuando quieras pasar de prototipo a sistema real, este HTML te sirve como especificación de diseño completa. La transición lógica:

1. **Stack ágil**: Next.js + Supabase + Vercel.
2. **Migración de datos**: el JSON que exporta `FINCO.exportJSON()` puede importarse a la base nueva.
3. **Integraciones críticas**: Google Calendar (sync de reuniones), WhatsApp Business API (recordatorios), MercadoPago (cobros), AFIP (facturación).
4. **Módulos que faltan**: Tareas/To-dos por cliente, biblioteca central de documentos, time tracking, reportes en PDF, templates reutilizables.
5. **Pestaña "Objetivos y métricas"** dentro de la ficha de cada cliente, con tracking mes a mes.

---

## Licencia

Uso interno de FINCO. Todos los derechos reservados.

---

*FINCO · Finanzas Conscientes · Mayo 2026*
