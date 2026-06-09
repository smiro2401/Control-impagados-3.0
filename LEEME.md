# Control de Impagados · Playoff (versión nube)

Aplicación web para controlar las facturas pendientes de tus clientes a partir
del Excel exportado de Holded. Muestra un ranking visual de deuda, KPIs,
distribución por tramos, estado de mora, prioridad de seguimiento por cliente
y una sección aparte para las facturas de Kit Digital.

Los datos se guardan en una base de datos central (Supabase), con acceso
protegido por email y contraseña, así que se ven desde cualquier equipo.

## Pasos (en orden)

1. CONFIGURACION.md ... configurar Supabase (crear proyecto, tablas, claves,
   usuario). Se hace una vez, ~10 min.
2. PUBLICAR-GITHUB.md . publicar la app en una URL pública con GitHub Pages.

## Contenido de la carpeta

- index.html .......... la aplicacion (versión nube, con login)
- CONFIGURACION.md .... configurar Supabase (leer primero)
- PUBLICAR-GITHUB.md .. publicar en GitHub Pages
- LEEME.md ............ este archivo

## Funciones

- KPIs en cabecera: deuda total, clientes con deuda, clientes críticos,
  antigüedad máxima y total de Kit Digital.
- Ranking de deuda con barras de intensidad segun importe.
- Distribución por tramos y dona por estado de mora.
- Listado de clientes ordenable y con buscador.
- Ficha por cliente: teléfono, email, persona de contacto, prioridad de
  seguimiento (estrellas) y notas. Se guarda en la nube.
- Prioridad visible como estrellas en el listado principal, ordenable.
- Facturas "Kit Digital" (subvención) separadas: no cuentan como deuda, van en
  su propia sección y en su propio KPI. Se detectan por el concepto.

## Actualización mensual

Cada carga de Excel reemplaza las facturas vigentes por las del fichero. Las
fichas de cliente (contacto, prioridad, notas) se conservan, cruzadas por
nombre de cliente.
