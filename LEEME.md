# Control de Impagados - Playoff

Aplicacion web para controlar facturas pendientes a partir del Excel exportado
de Holded.

## Plataforma

- Firebase Authentication: acceso con Google.
- Cloud Firestore: facturas y fichas de clientes.
- Firebase Hosting: publicacion web.
- Seguridad por usuario: cada cuenta ve unicamente sus datos.

## Puesta en marcha

Sigue `CONFIGURACION.md` para crear el proyecto Firebase, pegar la configuracion
y desplegar la aplicacion.

## Funciones

- KPIs de deuda, clientes criticos, antiguedad y Kit Digital.
- Ranking y distribucion visual de deuda.
- Busqueda y ordenacion de clientes.
- Ficha de contacto, prioridad y notas por cliente.
- Importacion del Excel de Holded.
- Separacion de facturas de Kit Digital.

Cada importacion reemplaza las facturas vigentes. Las fichas de cliente se
conservan.
