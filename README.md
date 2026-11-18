# cientyrecords.com

Sitio estático de Cienty Records LLC (HTML/CSS, sin build). Listo para GitHub Pages.

| Archivo | Qué es |
|---|---|
| `index.html` | Página principal |
| `privacidad.html` | Política de privacidad |
| `terminos.html` | Términos de servicio |
| `styles.css` | Estilos |
| `favicon.svg` | Ícono de pestaña |
| `404.html` | Página de error |
| `CNAME` | Le dice a GitHub Pages que el dominio es `cientyrecords.com` |
| `.nojekyll` | Publica los archivos tal cual, sin procesar con Jekyll |

## 1. Publicar en GitHub Pages

1. Crea un repositorio en GitHub (por ejemplo `cientyrecords-site`) y sube todos estos archivos a la raíz de la rama `main`.
2. En el repo: **Settings → Pages**.
   - *Source*: **Deploy from a branch**
   - *Branch*: `main` / `/ (root)` → **Save**
3. En **Custom domain** escribe `cientyrecords.com` y guarda (el archivo `CNAME` ya lo trae).

## 2. Verificar el dominio (recomendado)

Evita que otra cuenta de GitHub pueda tomar tu dominio.

1. GitHub → tu foto → **Settings → Pages → Add a domain** → `cientyrecords.com`.
2. GitHub te mostrará un registro **TXT** como:
   `_github-pages-challenge-TUUSUARIO` → `abc123...`
3. Créalo en tu proveedor de DNS y pulsa **Verify**.

## 3. Registros DNS

En el panel DNS de tu dominio (donde están hoy tus registros MX):

**Agrega** (Host `@` = el dominio raíz):

| Tipo | Host | Valor |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| AAAA | @ | 2606:50c0:8000::153 |
| AAAA | @ | 2606:50c0:8001::153 |
| AAAA | @ | 2606:50c0:8002::153 |
| AAAA | @ | 2606:50c0:8003::153 |
| CNAME | www | TUUSUARIO.github.io |

(`TUUSUARIO` = tu usuario u organización de GitHub, sin el nombre del repo.)

**Borra solo** los registros que choquen con los anteriores (si existen):
- Otros registros **A / AAAA** en `@` (p. ej. de una página de "parking" del registrador).
- Un registro propio en `www` (CNAME o A).
- Cualquier "URL forwarding / redirect" del registrador para el dominio raíz.

**Estado actual (Northwest, panel *Domains → cientyrecords.com → DNS Settings*):** ya están creados los 4 registros A en `@` y el CNAME `www → stephanbluitt.github.io.`. No había registro propio en `www`, solo un comodín `* → 66.223.49.89` de Northwest; se dejó intacto porque un registro específico (`www`) tiene prioridad sobre el comodín. HTTPS está activo y forzado.

**No toques** (son del correo):
- Registros **MX**.
- **TXT** de SPF (`v=spf1 ...`), DMARC (`_dmarc`) y verificación del proveedor de correo.
- **CNAME/TXT** de DKIM (p. ej. `google._domainkey`, `selector1._domainkey`).
- Cualquier CNAME de correo como `autodiscover`, `mail`, `webmail`.

Los registros A/AAAA/CNAME del sitio y los MX del correo son independientes: cambiar los primeros no afecta la entrega de correo.

> Si usas Cloudflare: deja los registros del sitio en **DNS only** (nube gris) hasta que GitHub emita el certificado HTTPS.

## 4. Activar HTTPS

Cuando el DNS propague (de minutos a 24 h), en **Settings → Pages** marca **Enforce HTTPS**. Si la casilla está deshabilitada, espera a que GitHub termine de emitir el certificado.

## 5. Comprobar

```bash
dig +short cientyrecords.com A
dig +short www.cientyrecords.com CNAME
dig +short cientyrecords.com MX
```

El primero debe mostrar las cuatro IPs `185.199.x.153`, el segundo `TUUSUARIO.github.io.` y el tercero tus MX de siempre, sin cambios.

> Si `dig` no muestra los registros nuevos, puede ser la caché de tu red (hasta 1 hora). Compruébalo en https://dns.google/resolve?name=cientyrecords.com&type=A

## Actualizar el sitio

Edita los archivos y súbelos a `main` (por ejemplo con `git push`). GitHub Pages publica los cambios en 1–2 minutos.
