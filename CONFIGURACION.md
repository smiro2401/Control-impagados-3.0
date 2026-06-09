# Configuración · Control de Impagados (versión nube)

Esta versión guarda las facturas y las fichas de clientes en una base de datos
central de Supabase (gratuita), con acceso protegido por email y contraseña.
Así ves los mismos datos desde cualquier ordenador.

La app es un único fichero (`index.html`), pero necesita conectarse a TU
proyecto de Supabase. La configuración se hace una sola vez y son unos 10
minutos. Sigue estos pasos en orden.

------------------------------------------------------------------------

## Paso 1 · Crear el proyecto Supabase

1. Entra en https://supabase.com y crea una cuenta (puedes usar tu cuenta de
   Google o un email). El plan gratuito es suficiente.
2. Pulsa **New project**.
3. Ponle un nombre (p. ej. `impagados-playoff`), elige una contraseña para la
   base de datos (guárdala, aunque la app no la usa directamente) y la región
   más cercana (Frankfurt / EU West).
4. Espera 1–2 minutos a que el proyecto se aprovisione.

------------------------------------------------------------------------

## Paso 2 · Crear las tablas

1. En el menú lateral de Supabase, abre **SQL Editor**.
2. Pulsa **New query**, pega TODO el bloque siguiente y pulsa **Run**:

```sql
-- Tabla de facturas (cada fila = una factura del Excel, en formato JSON)
create table if not exists public.invoices (
  id bigint generated always as identity primary key,
  data jsonb not null,
  created_at timestamptz default now()
);

-- Tabla de fichas de cliente (contacto y notas)
create table if not exists public.clients (
  client_key text primary key,
  data jsonb not null,
  updated_at timestamptz default now()
);

-- Seguridad: activar Row Level Security
alter table public.invoices enable row level security;
alter table public.clients  enable row level security;

-- Permitir todas las operaciones SOLO a usuarios autenticados (con login)
create policy "auth_all_invoices" on public.invoices
  for all to authenticated using (true) with check (true);

create policy "auth_all_clients" on public.clients
  for all to authenticated using (true) with check (true);
```

Debe aparecer "Success. No rows returned". Esto crea las dos tablas y deja
claro que solo quien haya iniciado sesión puede leer o escribir.

------------------------------------------------------------------------

## Paso 3 · Copiar tus dos claves en la app

1. En Supabase, ve a **Project Settings** (icono de engranaje abajo a la
   izquierda) → **API**.
2. Copia estos dos valores:
   - **Project URL** (algo como `https://xxxxxxxx.supabase.co`)
   - **Project API keys → anon public** (una cadena larga)
3. Abre `index.html` con un editor de texto (Bloc de notas, TextEdit, VS Code…).
4. Cerca del final, busca estas dos líneas:

   ```js
   const SUPABASE_URL = "PEGA_AQUI_TU_PROJECT_URL";
   const SUPABASE_ANON_KEY = "PEGA_AQUI_TU_ANON_KEY";
   ```

5. Sustituye el texto entre comillas por tus valores. Quedará algo así:

   ```js
   const SUPABASE_URL = "https://abcd1234.supabase.co";
   const SUPABASE_ANON_KEY = "eyJhbGciOiJI...";
   ```

6. Guarda el archivo.

> La `anon key` es pública por diseño: no da acceso a los datos por sí sola,
> porque las tablas exigen estar autenticado (lo configuraste en el paso 2).

------------------------------------------------------------------------

## Paso 4 · Crear tu usuario (login)

1. En Supabase, ve a **Authentication** → **Users** → **Add user** →
   **Create new user**.
2. Introduce tu email y una contraseña. Marca la opción de email confirmado
   (Auto Confirm User) si aparece, para no tener que validar por correo.
3. Repite para cada persona que deba acceder (p. ej. Sònia).

> Por seguridad, conviene mantener desactivado el registro abierto: en
> **Authentication → Providers → Email**, deja desactivado "Enable sign-ups"
> si quieres que solo tú puedas dar de alta usuarios desde el panel.

------------------------------------------------------------------------

## Paso 5 · Usar la app

- **En local:** abre `index.html` con doble clic. Te pedirá email y contraseña.
- **Publicada en una URL:** sube la carpeta a Netlify, Cloudflare Pages o
  similar (ver más abajo). La app pedirá login igualmente.

Una vez dentro: pulsa **Cargar Excel**, selecciona tu fichero de Holded y los
datos quedan guardados en la nube. Desde cualquier otro equipo, al entrar con
tu usuario, verás esas mismas facturas y fichas.

------------------------------------------------------------------------

## Publicar en una URL (opcional)

1. Ve a https://app.netlify.com/drop
2. Arrastra la carpeta completa (con el `index.html` ya configurado).
3. Netlify te da una URL pública. Como el acceso está protegido por login,
   solo entran los usuarios que diste de alta en el paso 4.

------------------------------------------------------------------------

## Cómo funciona la actualización mensual

Cada vez que cargas un Excel nuevo, la app **reemplaza** las facturas vigentes
por las del fichero (es el listado de impagados actual). Las **fichas de
cliente** (teléfono, email, notas) NO se borran: se conservan y se vuelven a
cruzar por el nombre del cliente.

------------------------------------------------------------------------

## Problemas frecuentes

- **"Configuración pendiente":** no pegaste bien las claves en el paso 3.
- **"Email o contraseña incorrectos":** revisa el usuario en Authentication →
  Users, o que la contraseña sea la correcta.
- **Entro pero no se guardan datos:** revisa que ejecutaste el SQL del paso 2
  completo (las tablas y las políticas). Sin las políticas, la base de datos
  rechaza lecturas y escrituras.
