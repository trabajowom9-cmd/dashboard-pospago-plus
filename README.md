# Uso de herramienta pospagos PLUS - Mall Ibagué

Dashboard ejecutivo (React + TypeScript) que **solo lee** tu Google Sheets y muestra KPIs, gráficos y tabla.
No escribe, no modifica y no inventa datos. Si la hoja está vacía muestra el mensaje de hoja vacía.

## Cómo lee la hoja
- Peticiones GET anónimas a la exportación CSV de la hoja (con respaldo gviz). **No hay claves ni credenciales** en el código.
- Requisito: la hoja debe estar compartida como **Cualquier persona con el enlace → Lector**. (Ya lo está: se verificó que se puede leer sin iniciar sesión. No se cambió ningún permiso.)
- Quien tenga el enlace de la hoja puede leerla; el dashboard no la hace más pública de lo que ya es.
- Estructura reconocida: bloques apilados (título como `KIOSCO LA ESTACION`, `ANDREA`, `NELSON` + encabezados `FECHA`, `PLUS TOTAL A LA FECHA`, `ACTIVOS CON PAGO`, `ACTIVOS SIN PAGO`, `PENDIENTES POR SER ACTIVADOS`). Si agregas una columna `META` u `OBJETIVO`, aparecen cumplimiento y brecha automáticamente. Nuevos bloques se detectan solos.
- Cambiar de hoja/pestaña: edita `src/config.ts` (`SHEET_ID`, `SHEET_GID`).

## Publicación gratuita (sin tarjeta)
Es un sitio estático (carpeta `dist/`), así que sirve cualquier hosting estático gratuito.

**Opción A – GitHub Pages (incluida)**
1. Crea una cuenta gratuita en github.com y un repositorio **público** (los repos privados con Pages requieren plan pago). El repo solo contiene código; el ID de la hoja es visible, pero no da más acceso que el enlace.
2. Sube todo el contenido de esta carpeta (sin `node_modules` ni `dist`).
3. Settings → Pages → Source: **GitHub Actions**.
4. El workflow `.github/workflows/deploy.yml` compila y publica. Tu enlace será `https://TU-USUARIO.github.io/NOMBRE-REPO/`.

**Opción B – Cloudflare Pages / Netlify (plan gratuito, cuenta sin tarjeta)**: conecta el repo, comando de build `npm run build`, carpeta de salida `dist`.

## Uso local
```
npm install
npm run dev      # http://127.0.0.1:5173
npm test         # pruebas unitarias
npm run build    # genera dist/
```
Requiere Node 20+.

## Estructura
- `src/data/` lectura (cliente, CSV, interpretación de la hoja)
- `src/lib/` cálculos y formato
- `src/hooks/` carga y actualización (30 s, sin solicitudes duplicadas, conserva últimos datos válidos)
- `src/components/` interfaz; `charts/` gráficos SVG propios (sin librerías de gráficos)
