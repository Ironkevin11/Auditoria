# Data & Apps — Ecosistema de Seguridad

Sitio estático de una sola página (`index.html`, sin build ni dependencias) más un workflow de GitHub Actions que corre un escaneo de **OWASP ZAP** en la nube, sin instalar nada localmente.

## 1. Subir el proyecto a GitHub

```bash
cd proyecto-zap-vercel
git init
git add .
git commit -m "Sitio Data & Apps + workflow ZAP"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/TU-REPO.git
git push -u origin main
```

## 2. Desplegar en Vercel

1. Entra a https://vercel.com y crea una cuenta (puedes usar tu cuenta de GitHub).
2. "Add New… → Project" → importa el repositorio que acabas de subir.
3. Como es HTML/CSS puro, Vercel lo detecta automáticamente (Framework Preset: "Other") y no pide configuración adicional.
4. Dale a "Deploy". En unos segundos te da una URL tipo `https://tu-repo.vercel.app`.

## 3. Correr el escaneo de OWASP ZAP (sin instalarlo)

El workflow `.github/workflows/zap-scan.yml` usa la Action oficial `zaproxy/action-baseline`, que ejecuta ZAP dentro de un runner de GitHub (Ubuntu en la nube), no en tu computador.

1. En GitHub, ve a la pestaña **Actions** del repo.
2. Entra al workflow **OWASP ZAP Baseline Scan** → **Run workflow**.
3. Pega la URL de Vercel que te dio el paso 2 en el campo `target_url` → **Run workflow**.
4. Cuando termina (2-5 min):
   - El reporte queda como **artifact** descargable (`zap_scan` → contiene `report_html.html`, `report_md.md`, `report_json.json`) en la misma corrida del workflow.
   - Además, si el escaneo encuentra alertas, ZAP abre/actualiza automáticamente un **Issue** en el repo con el detalle.

## 4. Para la entrega del viernes

- Descarga el `report_html.html` del artifact — es el reporte formal de ZAP con las alertas encontradas (severidad, descripción, cómo se explota, cómo mitigarlo).
- Puedes complementarlo explicando: qué probó ZAP (cabeceras de seguridad, cookies, XSS reflejado, etc.), cuáles alertas son reales riesgos para este sitio estático y cuáles no aplican por no tener backend.
