# project-spec.md

Construyo el Gestor de Pedidos de Conpro: una pantalla HTML independiente por línea de producto (café, frutos secos, y las que sigan) para registrar pedidos, clientes y márgenes de las compras colectivas de la comunidad, compartiendo una sola tabla de clientes y un solo proyecto Supabase entre todas las líneas. Es para uso exclusivo de Ricardo (sin login por persona). Stack: HTML+JS estático por producto (sin build), Supabase (Postgres + RLS) como backend compartido, desplegado en el mismo repo de GitHub y proyecto Vercel que ya usa CRL.

## Fuera del scope
- Caja ficticia por proveedor y su conciliación con la cartola del banco.
- Conciliación bancaria automática.
- Importación de pedidos por IA.
- Login o permisos por persona (hoy un solo usuario).
- Catálogo visual, carrito o descuento de stock en tiempo real para clientes (la plataforma completa, postergada según el informe de Escalamiento).
- Estadísticas/KPIs consolidados (quedan para después de este hito, cuando ya haya data real circulando por el modelo nuevo).

## Hitos
1. Crear la tabla `clientes` compartida en Supabase y migrar los clientes existentes de café (CRL) a ella, actualizando sus pedidos para que referencien la tabla nueva.
2. Adaptar la pantalla de café para que "Nuevo pedido" busque o cree clientes en la tabla compartida en vez de su tabla propia.
3. Construir el gestor de Frutos Secos (`fs_productos`, `fs_pedidos`) sobre la tabla de clientes compartida, con pantalla "Nuevo pedido" y exportación a Excel.
4. Verificar de punta a punta: un cliente que compra en ambas líneas aparece como el mismo cliente en los dos gestores.

Listo = con un cliente de prueba se puede registrar un pedido de café y uno de frutos secos, y en ambos gestores aparece como el mismo cliente, sin tocar las tablas de producto o pedido de la otra línea.
