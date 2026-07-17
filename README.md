# Cotizador interactivo — Ecosistema web integral

Herramienta de una sola página (`index.html`, sin backend ni build) para que el cliente construya su propia cotización a partir de los 13 módulos y ~130 características levantadas en el documento de requerimientos.

## Cómo usarlo

Abre `index.html` en cualquier navegador (doble clic o `open index.html`). No requiere servidor ni instalación.

- **Explorar**: clic en cualquier ficha para ver descripción, precio, prioridad, tiempo estimado y dependencias.
- **Ajustar el alcance**: arrastra fichas separables entre módulos, o al panel lateral "Para después" / "No me interesa". También puedes moverlas desde el detalle de la ficha (selector "Mover esta característica").
- **Módulos personalizados**: botón "+ Nuevo módulo" para agrupar necesidades propias del cliente, con sus propias características.
- **Cotización para el cliente**: genera un documento formal (HTML imprimible / descargable como PDF vía "Imprimir → Guardar como PDF") con los datos del cliente, el desglose por módulo y el total, listo para enviar.
- **Descargar resumen**: exporta un `.txt` con el resumen de la configuración actual.
- **Exportar datos**: exporta un `.json` con el estado completo de la cotización.
- **Restablecer**: vuelve la configuración a su estado original (los 13 módulos completos, USD 13,000).

Todo el progreso se guarda automáticamente en el `localStorage` del navegador (por dispositivo/navegador, no en la nube).

## Estructura

```
index.html   → aplicación completa (HTML + CSS + JS embebidos, sin dependencias externas)
docs/        → documento original de levantamiento de requerimientos y planificación modular
```

## Notas

- Precio total del alcance completo: USD 13,000 (13 módulos × USD 1,000).
- Los valores no incluyen hosting, dominio, pasarelas de pago y sus comisiones, consumo de IA, licencias, almacenamiento ni soporte posterior.
- La "cotización para el cliente" es un documento comercial (no un sistema de facturación fiscal ni de cobro).
