# CLAUDE.md

El qué vive en docs/project-spec.md, el dónde en docs/devlog.md.

## Convenciones
- Cada línea de producto es un archivo HTML independiente (`cafe.html`, `frutos-secos.html`, ...) en el mismo repo; no introducir un framework ni un build step para unificarlos.
- Todas las líneas comparten el mismo proyecto Supabase (`tgrwscyskqucsxndvyoa`) y la tabla `clientes`; las tablas de producto/pedido usan el prefijo de su línea (`fs_`, `cafe_`, ...) y referencian `clientes.id` por FK.
- RLS activado en toda tabla nueva desde que se crea, no después.
- Formato CLP en toda celda de precio: `$#,##0;-$#,##0;"-"` (mismo formato que los Excel de catálogo).
- El costo real de proveedor se confirma directo con el proveedor; nunca se usa el precio de lista/referencia público de conpro.cl como costo.

## Supabase
Antes de tocar el esquema o las políticas RLS, instalar y leer el skill de Supabase (`npx skills add supabase/agent-skills`).

## Cierre de sesión
Cuando diga "cierra la sesión", corre lint + types + tests; si pasan, agrega una entrada nueva arriba en docs/devlog.md (fecha, hito, qué hice, decisiones/bugs, próximo paso) y haz commit. Si algo falla, arréglalo o avísame antes de commitear. Nunca edites entradas viejas.

## Git
- Commits en español.
- No commitear `.env` ni ninguna key de Supabase, ni siquiera la anon/publishable.
- Un branch por hito; mergear a main solo cuando ese hito quede verificado de punta a punta.

## Lecciones
