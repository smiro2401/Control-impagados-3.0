# Publicar en GitHub Pages

Esta guía explica cómo poner la app online con una URL pública gratuita usando
GitHub Pages. La app sigue protegida por login (Supabase), así que aunque la
URL sea pública, solo entran los usuarios que tú des de alta.

> Antes de publicar, la app debe estar configurada con tus claves de Supabase
> (ver CONFIGURACION.md). Si no, al abrir la URL solo verás "configuración
> pendiente".

------------------------------------------------------------------------

## Opción A · Por la web (sin instalar nada, recomendada)

1. Crea una cuenta en https://github.com si no la tienes.
2. Pulsa el botón **+** (arriba a la derecha) → **New repository**.
   - Repository name: por ejemplo `impagados`.
   - Visibilidad: **Public** (GitHub Pages gratuito requiere repo público;
     no pasa nada porque el acceso a datos está protegido por login, ver nota
     de seguridad abajo).
   - Marca "Add a README file".
   - **Create repository**.
3. Dentro del repo, pulsa **Add file** → **Upload files**.
4. Arrastra el archivo `index.html` (ya configurado con tus claves). Si quieres,
   sube también los `.md`. Pulsa **Commit changes**.
5. Ve a **Settings** (del repositorio) → en el menú izquierdo, **Pages**.
6. En **Build and deployment** → **Source**, elige **Deploy from a branch**.
   En **Branch** selecciona `main` y carpeta `/ (root)`. Pulsa **Save**.
7. Espera 1–2 minutos. GitHub te mostrará la URL pública, del tipo:
   `https://TU-USUARIO.github.io/impagados/`
8. Abre esa URL: te pedirá login. Entra con el usuario que creaste en Supabase.

Para actualizar la app en el futuro (por ejemplo, una versión nueva que te
pase), repite el paso 3–4 subiendo el `index.html` nuevo (sobrescribe el
anterior) y en 1–2 minutos la URL queda actualizada.

------------------------------------------------------------------------

## Opción B · Con Git desde tu ordenador (si te manejas con terminal)

```bash
# en la carpeta que contiene index.html
git init
git add index.html CONFIGURACION.md LEEME.md PUBLICAR-GITHUB.md
git commit -m "App control de impagados"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/impagados.git
git push -u origin main
```

Luego activa Pages igual que en los pasos 5–7 de la opción A.

------------------------------------------------------------------------

## Nota de seguridad importante

- **El repositorio será público**, así que el `index.html` (incluida tu
  `anon key` de Supabase) lo puede ver cualquiera. Esto es correcto y seguro
  por diseño: la `anon key` está pensada para ir en el navegador y NO da acceso
  a los datos por sí sola, porque tus tablas exigen estar autenticado (lo
  configuraste con las políticas RLS del paso 2 de CONFIGURACION.md).
- **Nunca subas a GitHub la `service_role key`** de Supabase (esa sí es secreta
  y da acceso total). La app no la usa; solo usa la `anon key`. Si en algún
  momento copiaste la clave equivocada, cámbiala.
- Para reforzar: en Supabase, **Authentication → Providers → Email**, deja
  desactivado "Enable sign-ups" para que nadie pueda auto-registrarse desde la
  URL pública. Solo tú das de alta usuarios desde el panel.
- En Supabase, **Authentication → URL Configuration**, puedes añadir tu URL de
  GitHub Pages (`https://TU-USUARIO.github.io`) a los Site URL / Redirect URLs
  permitidos.

------------------------------------------------------------------------

## Alternativa: repositorio privado

Si prefieres que el código no sea visible públicamente, GitHub Pages desde
repos privados requiere un plan de pago (GitHub Pro / Team). En ese caso, otra
opción gratuita con repos privados es **Cloudflare Pages** o **Netlify**, que
conectan con tu repo de GitHub privado y publican el sitio. El resultado para
el usuario es el mismo: una URL con login.
