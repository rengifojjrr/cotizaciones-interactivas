# Cotizador interactivo — Ecosistema web integral

Herramienta de una sola página (`index.html`, sin build) para que el cliente construya su propia cotización a partir de los 13 módulos y ~130 características levantadas en el documento de requerimientos. Cada cotización se guarda en Supabase con un link único por cliente.

**Sitio publicado:** https://rengifojjrr.github.io/cotizaciones-interactivas/

## Cómo compartir una cotización con un cliente

1. Abre el sitio publicado (sin parámetros en la URL). La app genera automáticamente un ID único y lo agrega a la barra de direcciones (`?q=...`).
2. Clic en "🔗 Copiar enlace de esta cotización" y envía ese link al cliente.
3. Ese link es *de ese cliente*: cualquiera que lo abra ve y puede modificar exactamente esa cotización (no hay login, funciona como un link de Google Docs — trátalo como semi-privado). Cada cliente nuevo necesita su propio link: vuelve a abrir el sitio sin `?q=` para generar uno nuevo.
4. Los cambios se guardan automáticamente en Supabase (ver el indicador "☁️ Guardado en la nube" junto al logo). Si el cliente recarga la página, cierra el navegador o vuelve otro día con el mismo link, su cotización sigue ahí.

## Funciones dentro de la app

- **Explorar**: clic en cualquier ficha para ver descripción, precio, prioridad, tiempo estimado y dependencias.
- **Ajustar el alcance**: arrastra fichas separables entre módulos, o al panel lateral "Para después" / "No me interesa". También puedes moverlas desde el detalle de la ficha (selector "Mover esta característica").
- **Módulos personalizados**: botón "+ Nuevo módulo" para agrupar necesidades propias del cliente, con sus propias características.
- **Vista previa: solo lo obligatorio**: casilla debajo del buscador que, sin mover ni cambiar nada de la cotización real, marca visualmente las características no obligatorias como excluidas y muestra el total y las horas estimadas de desarrollo de quedarse solo con lo obligatorio. Es una simulación de solo lectura (no se guarda ni afecta la cotización real).
- **Cotización para el cliente**: genera un documento formal (HTML imprimible / descargable como PDF vía "Imprimir → Guardar como PDF") con los datos del cliente, el desglose por módulo y el total, listo para enviar.
- **Descargar resumen**: exporta un `.txt` con el resumen de la configuración actual.
- **Exportar datos**: exporta un `.json` con el estado completo de la cotización.
- **Restablecer**: vuelve la configuración a su estado original (los 13 módulos completos, USD 13,000).

## Cómo correrlo en local

Abre `index.html` directamente en el navegador (doble clic o `open index.html`). No requiere servidor propio: el backend es Supabase, ya está conectado desde el HTML.

## Panel de administración (historial de cambios)

`admin.html` es una página aparte, sin link desde la app del cliente, donde el equipo puede ver **todas** las cotizaciones creadas y el historial completo de movimientos de cada una (qué característica se movió, de dónde a dónde, cuándo, quién creó un módulo personalizado, cuándo se restableció, cuándo el cliente descargó su cotización formal, etc.). Así queda registro aunque el cliente cambie algo y después lo vuelva a poner como estaba.

**Acceso:** https://rengifojjrr.github.io/cotizaciones-interactivas/admin.html — entras con tu correo autorizado (sin contraseña, se envía un enlace de acceso al correo). Solo `rengifojjrr@gmail.com` está autorizado por ahora; para agregar más correos del equipo hay que actualizar la política en Supabase (ver abajo).

**Configuración pendiente (una sola vez):** para que el enlace de acceso funcione, hay que autorizar la URL del panel en Supabase:
1. Entra a https://supabase.com/dashboard/project/vajbsfgojtunamhrzrpf/auth/url-configuration
2. En "Redirect URLs" agrega: `https://rengifojjrr.github.io/cotizaciones-interactivas/admin.html`
3. Guarda. Sin este paso, Supabase no te dejará entrar aunque el correo esté autorizado.

## Backend (Supabase)

- Proyecto: `cotizaciones-interactivas` (organización de Supabase del equipo).
- Tabla `public.quotes`: `id uuid` (uno por cotización/cliente), `client jsonb`, `quote_state jsonb`, timestamps.
- Tabla `public.quote_events`: historial append-only de acciones (`quote_id`, `event_type`, `summary`, `detail jsonb`, `created_at`).
- RLS está activo y **sin políticas públicas** sobre ninguna de las dos tablas: nadie puede leer ni listar filas directamente vía la API con la clave pública. Todo el acceso del cliente pasa por tres funciones `SECURITY DEFINER` expuestas como RPC:
  - `get_quote(p_id uuid)` — devuelve una cotización por su ID exacto.
  - `upsert_quote(p_id uuid, p_client jsonb, p_state jsonb)` — crea o actualiza una cotización por ID.
  - `log_quote_event(p_quote_id uuid, p_type text, p_summary text, p_detail jsonb)` — agrega una entrada al historial (solo inserta, nunca permite leer).
- Esto evita que alguien con la clave pública (anon key, embebida en el HTML — es normal que sea pública) pueda enumerar o listar las cotizaciones o el historial de otros clientes; solo puede operar sobre un ID que ya conoce (el que viene en su link).
- La **lectura** de `quotes` y `quote_events` (para el panel de administración) solo está permitida a usuarios autenticados por Supabase Auth cuyo correo esté en la lista blanca (`rengifojjrr@gmail.com`), sin importar si iniciaron sesión con ese correo o cualquier otro — un correo fuera de la lista simplemente no ve datos.
- El guardado hacia Supabase tiene un debounce de ~700ms tras cada cambio. Si no hay conexión, el indicador cambia a "⚠️ Sin conexión" y el cambio queda respaldado en `localStorage` del navegador hasta que vuelva la conexión y se edite algo de nuevo. El historial (`quote_events`) se registra al momento de cada acción, no con debounce.

## Estructura

```
index.html   → aplicación completa (HTML + CSS + JS, con supabase-js vía CDN)
admin.html   → panel privado con el listado de cotizaciones y su historial de cambios
docs/        → documento original de levantamiento de requerimientos y planificación modular
```

## Notas

- Precio total del alcance completo: USD 13,000 (13 módulos × USD 1,000).
- Los valores no incluyen hosting, dominio, pasarelas de pago y sus comisiones, consumo de IA, licencias, almacenamiento ni soporte posterior.
- La "cotización para el cliente" es un documento comercial (no un sistema de facturación fiscal ni de cobro).
- Los precios y el desglose de módulos son visibles para cualquiera con el link de una cotización (es una app del lado del cliente); no incluyas ahí información que no quieras que el cliente vea.
