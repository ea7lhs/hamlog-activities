# 📻 HamLog — Sistema de publicaciones para radioaficionados

Web estática para publicar logs, noticias, eventos y galerías. Sin backend propio. Gratis en GitHub Pages.

## Tipos de contenido

- 📡 **Actividad con log** — ADIF + estadísticas + generador de tarjetas QSL
- 📝 **Noticia / Blog** — Artículo con editor visual (Markdown transparente)
- 📅 **Evento** — Convocatoria con cuenta atrás en tiempo real
- 🖼️ **Galería** — Álbum de fotos con lightbox

---

## ⚙️ Configuración inicial (una sola vez)

### 1. Fork del repositorio

Haz fork de este repo a tu cuenta de GitHub.

### 2. Activa GitHub Pages

1. Settings → Pages
2. Source: **Deploy from a branch** → `main` / `/ (root)`
3. Tu web estará en `https://TU_USUARIO.github.io/TU_REPO`

### 3. Configura `hamlog.config.json`

```json
{
  "callsign": "EA7LHS",
  "titulo": "HamLog",
  "subtitulo": "Actividades y noticias"
}
```

### 4. Actualiza las constantes en los archivos HTML

En `index.html`, `actividad.html`, `post.html`, `evento.html`, `galeria.html` y `admin.html` busca y rellena:

```js
const GITHUB_USER = 'TU_USUARIO';
const GITHUB_REPO = 'TU_REPO';
```

### 5. Configura Clerk (login del panel admin)

Clerk gestiona el login con GitHub para el panel de administración. Es gratuito.

1. Crea cuenta en https://clerk.com
2. Crea una nueva aplicación
3. En "Social connections" activa **GitHub**
4. En GitHub: ve a https://github.com/settings/developers → OAuth Apps → New OAuth App
   - Homepage URL: `https://TU_USUARIO.github.io/TU_REPO`
   - Callback URL: la que te da Clerk en su panel
5. Copia las credenciales de GitHub a Clerk
6. En Clerk → API Keys → copia la **Publishable Key** (`pk_live_...`)
7. Pégala en `admin.html`:

```js
const CLERK_KEY = 'pk_live_XXXXXXXXXX';
```

> **Sin Clerk configurado**, el panel funciona igualmente en modo desarrollo: pide un token de GitHub personal al entrar. Útil para probar.

---

## 📋 Estructura de carpetas

```
/actividades/nombre-actividad/
    info.json       ← datos de la actividad
    log.adi         ← log ADIF
    foto.jpg        ← foto de portada
    diploma.png     ← imagen QSL (opcional)

/posts/nombre-post/
    info.json

/eventos/nombre-evento/
    info.json

/galerias/nombre-galeria/
    info.json
    foto1.jpg
    foto2.jpg
    ...
```

El panel de administración crea esta estructura automáticamente.

---

## 📋 Formato de `info.json`

### Actividad con log
```json
{
  "nombre": "POTA EA5-001",
  "tipo": "POTA",
  "fecha": "2025-05-15",
  "indicativo": "EA7LHS",
  "descripcion": "Descripción breve",
  "foto": "foto.jpg"
}
```

### Noticia / Evento / Galería
```json
{
  "titulo": "Título",
  "fecha": "2025-06-01",
  "descripcion": "Resumen corto",
  "contenido": "Texto en **Markdown**",
  "foto": "portada.jpg",
  "fotos": ["foto1.jpg","foto2.jpg"]
}
```

---

## 73 📻
