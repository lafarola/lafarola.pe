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

> ⚠️ NUNCA llamar `quarto.cmd` por ruta completa: el .cmd se rompe con el
> espacio de "Program Files" (error "Module not found ... deno.exe").
> **Arreglado 17-ago**: la carpeta de quarto ya está en el PATH de usuario -
> abrir una TERMINAL NUEVA y `quarto` funciona a secas.

```powershell
cd C:\Users\gonzalo.urbina\Documents\planning\sitio
quarto preview
```

(O abrir `sitio/` como proyecto en RStudio y usar el botón Render.)

## 2. Crear el repo en GitHub ✅ HECHO (por Gonzalo)

Repo: **github.com/lafarola/lafarola.pe** (el handle real es `lafarola`,
no `la-farola` como decía el boilerplate - ya corregido allí también).

## 3. Conectar y pushear ✅ HECHO (por Gonzalo)

⚠️ Si Windows tiene guardadas credenciales de otra cuenta GitHub, el push
puede salir autenticado con esa. Los COMMITS ya van como La Farola (config
local), pero para que el push también sea de la cuenta correcta: Panel de
control → Administrador de credenciales → Credenciales de Windows → borrar
`git:https://github.com` y volver a autenticarse cuando git lo pida.

## 4. Publicar el sitio - QUEDA UN SOLO COMANDO (17-ago)

Estado: la rama `gh-pages` ya existe y tiene el sitio COMMITEADO en local
(Claude no puede pushear desde su sesión - el credential manager exige
terminal interactiva). Falta solo, en TU terminal:

```powershell
cd C:\Users\gonzalo.urbina\Documents\planning\sitio
git push origin gh-pages main
```

(El `main` extra sube también las correcciones de este documento.)

Para republicar en el futuro tras cualquier cambio (terminal nueva, con el
PATH ya arreglado):

```powershell
quarto publish gh-pages
```

(Renderiza + empuja; aceptar el prompt. Además commit+push de `main` para
guardar la fuente.)

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
| CNAME | `www`  | `lafarola.github.io`    |

## 6b. Mismo viaje a Cloudflare: recibir correo en @lafarola.pe

Hoy el dominio solo ENVÍA (MailerLite). Para que lo que llegue a
gonzalo@lafarola.pe caiga en el Gmail (gratis, Cloudflare Email Routing):

1. Dashboard → zona lafarola.pe → **Email** → **Email Routing** → Enable.
2. **Destination address**: la.farola.pe@gmail.com → Cloudflare manda un
   correo de verificación al Gmail → click en verificar.
3. **Custom address**: gonzalo@lafarola.pe → Action "Send to" → el Gmail
   verificado. (Opcional: Catch-all → Send to el mismo Gmail, para que
   hola@/info@ también lleguen.)
4. Cloudflare ofrece agregar los DNS solo (3 registros MX
   route1/2/3.mx.cloudflare.net + 1 TXT SPF). Aceptar, con UNA precaución:
   ⚠️ **si ya existe un TXT que empieza `v=spf1` en la raíz** (del setup de
   MailerLite), NO puede haber dos - se FUSIONAN en uno solo, p. ej.:
   `v=spf1 include:<lo-de-mailerlite> include:_spf.mx.cloudflare.net ~all`
   (dos registros SPF separados = SPF inválido y la entregabilidad del
   remitente autenticado se rompe).
5. Probar: mandarse un correo a gonzalo@lafarola.pe desde otra cuenta.

Nota: Email Routing solo RECIBE/reenvía. Responder DESDE gonzalo@lafarola.pe
en Gmail es otro paso opcional ("Send mail as" + app password); hoy no hace
falta - el reply-to de las campañas ya es el Gmail.

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
