# 📻 HamLog — Sistema de logs para radioaficionados

Web estática para publicar logs y actividades de radioafición. Sin backend, sin base de datos. Se despliega gratis en GitHub Pages.

## Estructura del repo

```
hamlog/
├── index.html              ← Web pública (listado de actividades)
├── actividad.html          ← Visor de log + generador QSL
├── admin.html              ← Panel de administración
├── hamlog.config.json      ← Configuración del sitio
├── actividades/
│   └── nombre-actividad/
│       ├── info.json       ← Datos de la actividad
│       ├── log.adi         ← Log en formato ADIF
│       ├── foto.jpg        ← Foto de portada
│       └── diploma.png     ← Imagen base para QSL (opcional)
└── .github/workflows/
    └── deploy.yml          ← Auto-deploy a GitHub Pages
```

---

## ⚙️ Configuración inicial (una sola vez)

### 1. Fork del repositorio

Haz fork de este repo a tu cuenta de GitHub.

### 2. Edita `index.html` y `actividad.html`

En **ambos archivos**, busca estas líneas al final del `<script>` y rellena:

```js
const GITHUB_USER   = 'TU_USUARIO';   // tu usuario de GitHub
const GITHUB_REPO   = 'TU_REPO';      // nombre del repo
const GITHUB_BRANCH = 'main';
```

### 3. Edita `admin.html`

Busca las mismas líneas y además:

```js
const OAUTH_CLIENT_ID = 'TU_CLIENT_ID';  // ver paso 4
```

### 4. Crea la OAuth App de GitHub

1. Ve a https://github.com/settings/developers
2. → "OAuth Apps" → "New OAuth App"
3. Rellena:
   - **Application name:** HamLog Admin
   - **Homepage URL:** `https://TU_USUARIO.github.io/TU_REPO`
   - **Authorization callback URL:** `https://TU_USUARIO.github.io/TU_REPO/admin.html`
4. Clic en "Register application"
5. Copia el **Client ID** y pégalo en `admin.html` (paso 3)

> **Nota sobre el proxy OAuth:** GitHub no permite intercambiar el código OAuth directamente desde el navegador. El `admin.html` usa un proxy público para esto. Para producción seria, despliega tu propio proxy en Cloudflare Workers o Vercel (ver sección avanzada abajo).
>
> **Alternativa más simple:** Si prefieres no configurar OAuth, elimina `OAUTH_CLIENT_ID` y el panel pedirá un token personal de GitHub (Settings → Developer settings → Personal access tokens → Fine-grained). El admin lo pega una vez y se guarda en el navegador.

### 5. Configura `hamlog.config.json`

```json
{
  "callsign": "EA7LHS",
  "titulo": "Mis actividades de radioafición"
}
```

### 6. Activa GitHub Pages

1. Ve a Settings → Pages en tu repo
2. Source: **GitHub Actions**
3. La web estará en `https://TU_USUARIO.github.io/TU_REPO`

---

## 📋 Formato de `info.json`

```json
{
  "nombre": "POTA EA5-001 · Mayo 2025",
  "tipo": "POTA",
  "fecha": "2025-05-15",
  "indicativo": "EA7LHS",
  "descripcion": "Descripción opcional de la actividad.",
  "foto": "foto.jpg"
}
```

Campos `tipo` válidos: `POTA`, `SOTA`, `CONCURSO`, `EXPEDICIÓN`, `EVENTO`, `ACTIVIDAD`

---

## 🔧 Proxy OAuth avanzado (opcional)

Para no depender del proxy público, despliega este worker en Cloudflare (gratis):

```js
// worker.js
export default {
  async fetch(req) {
    const url = new URL(req.url);
    const code = url.searchParams.get('code');
    const r = await fetch('https://github.com/login/oauth/access_token', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
      body: JSON.stringify({
        client_id: 'TU_CLIENT_ID',
        client_secret: 'TU_CLIENT_SECRET',
        code
      })
    });
    const data = await r.json();
    return Response.json({ token: data.access_token });
  }
};
```

Luego en `admin.html` cambia la URL del proxy por la de tu worker.

---

## 73 de EA7LHS 📻
