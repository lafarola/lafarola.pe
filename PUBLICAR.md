# Publicar lafarola.pe — pasos de Gonzalo

> Sitio Quarto → GitHub Pages → dominio lafarola.pe (Cloudflare DNS).
> Sin GitHub Actions: se publica desde esta máquina con `quarto publish`.
> ⚠️ REGLA: todo lo publicado va autoreado por la cuenta GitHub de La Farola
> ÚNICAMENTE (el git config local de esta carpeta ya está fijado a
> `La Farola <la.farola.pe@gmail.com>` — los commits salen bien solos).

## 0. Antes de publicar (una sola cosa)

- [ ] Verificar la anécdota de Cusco en
  `posts/2026-08-el-codigo-se-copia-y-pega/index.qmd` — que esté contada
  como pasó de verdad. Es el mismo texto del email 1: si cambias algo aquí,
  cambiarlo también en `talleres/PANEL_DATA/emails_lanzamiento.md`.

## 1. Ver el sitio en local (opcional pero recomendado)

Quarto no está en el PATH; usar la ruta completa (viene con RStudio):

```powershell
cd C:\Users\gonzalo.urbina\Documents\planning\sitio
& "C:\Program Files\RStudio\resources\app\bin\quarto\bin\quarto.cmd" preview
```

(O abrir `sitio/` como proyecto en RStudio y usar el botón Render.)

## 2. Crear el repo en GitHub (como la cuenta La Farola)

1. Iniciar sesión en github.com **con la cuenta de La Farola** (según
   `documentos/boilerplate_marca.md` el handle es `la-farola` — si es otro,
   ajustar las URLs de abajo).
2. New repository → nombre **`lafarola.pe`** → público → SIN README ni
   .gitignore (el repo local ya los tiene).

## 3. Conectar y pushear

```powershell
cd C:\Users\gonzalo.urbina\Documents\planning\sitio
git remote add origin https://github.com/la-farola/lafarola.pe.git
git push -u origin main
```

⚠️ Si Windows tiene guardadas credenciales de otra cuenta GitHub, el push
puede salir autenticado con esa. Los COMMITS ya van como La Farola (config
local), pero para que el push también sea de la cuenta correcta: Panel de
control → Administrador de credenciales → Credenciales de Windows → borrar
`git:https://github.com` y volver a autenticarse cuando git lo pida.

## 4. Publicar el sitio

```powershell
& "C:\Program Files\RStudio\resources\app\bin\quarto\bin\quarto.cmd" publish gh-pages
```

Renderiza y empuja la rama `gh-pages`. Aceptar el prompt la primera vez.
Para republicar después de cualquier cambio: mismo comando (más commit+push
de `main` para guardar la fuente).

## 5. Activar Pages + dominio (una sola vez, en GitHub)

Repo → Settings → Pages:

1. Confirmar Source = rama `gh-pages` (quarto la creó).
2. Custom domain: `lafarola.pe` → Save. Esperar el check de DNS
   (necesita el paso 6 hecho).
3. Marcar **Enforce HTTPS** cuando se habilite (el certificado puede tardar
   de minutos a unas horas).

## 6. DNS en Cloudflare (una sola vez)

Zona lafarola.pe → DNS → Records. **No tocar los registros de MailerLite
(TXT/CNAME de autenticación del correo).** Agregar, todos en modo
**DNS only (nube gris)** — GitHub Pages necesita emitir su propio
certificado y el proxy naranja lo estorba:

| Tipo  | Nombre | Contenido               |
|-------|--------|-------------------------|
| A     | `@`    | `185.199.108.153`       |
| A     | `@`    | `185.199.109.153`       |
| A     | `@`    | `185.199.110.153`       |
| A     | `@`    | `185.199.111.153`       |
| CNAME | `www`  | `la-farola.github.io`   |

## 7. Verificar

- https://lafarola.pe carga el home con candado (HTTPS).
- https://lafarola.pe/blog.html lista el post 1.
- El RSS existe: https://lafarola.pe/blog.xml.

## Pendientes del sitio (no bloquean el estreno)

- [ ] Favicon: requiere la variante silueta sólida del logo (el crosshatch
  se empasta bajo 60px — pendiente del brand book). Al tenerla:
  `favicon: assets/...` en `_quarto.yml`.
- [ ] Formulario de suscripción MailerLite embebido (home + fin de cada
  post) — hoy la captura de correos sigue vía Thinkific/WhatsApp.
- [ ] Google Search Console (verificar dominio, enviar sitemap
  `https://lafarola.pe/sitemap.xml`) — SEO es canal de descubrimiento.
- [ ] Posts 2+ del calendario (`blog/CALENDARIO.md`); el post 4 (paquete)
  sigue bloqueado por el push del repo `lafarola`.
- [ ] Al estar el sitio vivo: actualizar links en bios/perfiles (FB/IG,
  LinkedIn, Thinkific) hacia lafarola.pe.
