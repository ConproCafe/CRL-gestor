# project-spec.md

Construyo el Gestor de Pedidos de Conpro: una pantalla HTML independiente por línea de producto (café en `index.html`, frutos secos en `frutos-secos.html`, y las que sigan) para registrar pedidos, clientes y márgenes de las compras colectivas de la comunidad, reutilizando el mismo proyecto Supabase y las mismas tablas genéricas (`proveedores`, `catalogo`, `clientes`, `pedidos`) que ya usa el gestor de café en producción — no se crean tablas nuevas por línea. Es para uso exclusivo de Ricardo (sin login por persona). Stack: HTML+JS estático por producto (sin build), Supabase (Postgres + RLS) como backend compartido, desplegado en el mismo repo de GitHub y proyecto Vercel que ya usa CRL.

## Fuera del scope
- Caja ficticia por proveedor y su conciliación con la cartola del banco.
- Conciliación bancaria automática.
- Importación de pedidos por IA (el "pegar texto de WhatsApp" del gestor de café no se replica en frutos secos; carga 100% manual).
- Login o permisos por persona (hoy un solo usuario).
- Catálogo visual, carrito o descuento de stock en tiempo real para clientes (la plataforma completa, postergada según el informe de Escalamiento).
- Estadísticas/KPIs consolidados entre líneas de producto (quedan para después de este hito, cuando ya haya data real circulando por ambas líneas).
- Tocar o refactorizar `index.html` (café) salvo que un bug lo requiera — el gestor de café está en producción con datos reales y no se arriesga para construir frutos secos.

## Hitos
1. Insertar a Enuss como proveedor nuevo en la tabla `proveedores` (`tipo_margen='fijo'`, formato "1 kg") y cargar su catálogo confirmado en `catalogo` con ese `proveedor_id` (reusando `kp`=costo real del proveedor, `km`=precio de venta a la comunidad).
2. Construir `frutos-secos.html`: una versión recortada de `index.html` sin molienda, sin presentaciones de café (kg/2×500/250g) ni el importador de WhatsApp, que reutiliza las mismas tablas `clientes`/`catalogo`/`pedidos`/`proveedores` vía Supabase.
3. Al guardar un pedido, generar dos documentos de salida propios de Frutos Secos:
   - **Orden de Compra a Enuss**, un único bloque de texto con dos secciones: arriba la tabla de control interno (Producto / Cantidad Kg / Precio Unitario / Total, sin desglose de IVA), abajo — separada por una línea — la interpretación en texto plano que Enuss pide para ingresar el pedido en su sistema, una línea por producto en formato `{cantidad}kg {producto}  ${precio}*{cantidad}` (siempre con "$", espaciado parejo entre todas las líneas). Dos botones de copiar: "Copiar registro completo" y "Copiar pedido para Enuss" (solo el bloque de texto plano).
   - **Resumen de cobro por cliente**, agrupado por persona, con Producto / Precio Conpro / Precio Final a Pagar, subtotal por persona y total del pedido.
4. Verificar de punta a punta: registrar un pedido de prueba con 2+ clientes y 2+ productos; confirmar que un cliente que también compra café aparece como el mismo cliente (tabla `clientes` compartida); y que los totales de la OC a Enuss y del resumen de cobro por cliente cuadran entre sí.

Listo = se puede registrar un ciclo de pedidos de frutos secos de principio a fin en `frutos-secos.html`, y de ahí salen directo la OC para Enuss y el resumen de cobro por cliente, sin pasar por Excel armado a mano.
