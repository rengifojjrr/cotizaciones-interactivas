# Levantamiento de proyecto — plantilla para cotizar

> **Para el asistente que rellena este archivo:** este documento se entrega a un equipo de
> desarrollo que lo va a convertir en una cotización real, con fases, costos y un PDF que verá
> el cliente. Rellena todo lo que puedas con la información de esta conversación. **No inventes
> datos** — si algo no se habló, escribe `POR DEFINIR` y sigue. Es mucho mejor un campo marcado
> como pendiente que un dato inventado que después hay que desmentir frente al cliente.
>
> Devuelve el archivo completo, con la misma estructura y los mismos títulos. No borres secciones
> aunque queden vacías.

---

## 1. Cliente

| Campo | Valor |
|---|---|
| Nombre del cliente | |
| Empresa | |
| Persona de contacto | |
| Correo | |
| Teléfono / WhatsApp | |
| ¿Es un cliente nuevo o ya trabajamos con él? | |
| Cómo llegó / contexto | |

## 2. El proyecto

| Campo | Valor |
|---|---|
| Nombre del proyecto | |
| Nombre corto en mayúsculas (ej. `TIENDA-F1`) | |
| ¿Qué es, en una frase? | |
| ¿Qué problema le resuelve al cliente? | |
| ¿Quién lo va a usar? (tipos de usuario) | |
| ¿Hay algo ya hecho o se empieza de cero? | |
| Fecha o plazo que pidió el cliente | |

**Resumen para el cliente** (3-6 frases, en lenguaje sencillo, tuteando; esto se muestra tal cual
en su portal, así que evita tecnicismos):

```
```

## 3. Presupuesto

| Campo | Valor |
|---|---|
| Presupuesto aprobado o rango | |
| Moneda | USD |
| ¿El monto es cerrado o hay margen? | |
| ¿Hay algo que el cliente ya dijo que NO quiere pagar ahora? | |

> **Si conoces el presupuesto**, reparte los costos entre los entregables de la sección 4 y
> asegúrate de que la suma dé exactamente ese número.
> **Si no lo conoces**, deja la columna de costo vacía y describe bien el alcance: los precios
> se ponen después.

## 4. Fases y entregables

> Divide el proyecto en fases (normalmente entre 3 y 8). Una fase es un bloque que se puede
> entregar y mostrar por separado. Dentro de cada fase, lista entre 3 y 6 entregables concretos.
>
> - **Obligatorio**: sin esto la fase no funciona ni tiene sentido entregarla.
> - **Opcional**: mejora el resultado, pero el cliente puede quitarlo para bajar el costo.
>
> **Sobre el tiempo estimado:** pon un rango de horas por entregable (ej. "6 a 8 horas"). El
> desarrollo se hace con asistencia de IA, así que estima sobre esa base y no sobre horas de
> trabajo manual tradicional: en la práctica es cerca de la mitad.
>
> Duplica el bloque de abajo por cada fase.

### Fase 1 — [nombre de la fase]

- **Qué se logra en esta fase:**
- **Qué NO entra en esta fase:** (sé explícito; esto evita discusiones después)
- **Cuándo se hace:** (ej. "Días 1 a 4", "Semana 2")

| Entregable | Qué incluye | Costo | Obligatorio / Opcional | Tiempo estimado |
|---|---|---|---|---|
| | | | | |
| | | | | |
| | | | | |

### Fase 2 — [nombre de la fase]

- **Qué se logra en esta fase:**
- **Qué NO entra en esta fase:**
- **Cuándo se hace:**

| Entregable | Qué incluye | Costo | Obligatorio / Opcional | Tiempo estimado |
|---|---|---|---|---|
| | | | | |
| | | | | |
| | | | | |

## 5. Comprobación de números

> Rellena esto tú mismo antes de devolver el archivo. Si no cuadra, corrige los costos de la
> sección 4 (no el total).

| Concepto | Monto |
|---|---|
| Suma de todos los entregables **obligatorios** | |
| Suma de todos los entregables **opcionales** | |
| **Total (obligatorios + opcionales)** | |
| ¿Coincide con el presupuesto de la sección 3? | Sí / No |

## 6. Tiempos de entrega

| Campo | Valor |
|---|---|
| Tiempo estimado de entrega completo | |
| ¿Qué puede retrasar el proyecto? | |
| ¿Hay una fecha tope real? (evento, lanzamiento, temporada) | |

**Lo que necesitamos del cliente para cumplir el plazo** (una fila por cosa; incluye cuánto tarda
cada trámite si se sabe — por ejemplo, habilitar una pasarela de pago suele tardar días):

| Qué se necesita | Por qué y cuánto tarda | Quién lo gestiona |
|---|---|---|
| | | |
| | | |

## 7. Lo que NO incluye el presupuesto

> Todo lo que el cliente podría dar por sentado y no está cubierto: hosting, dominio, comisiones
> de pasarelas, licencias, fotografía, redacción, soporte posterior, app móvil, multi-idioma,
> integraciones con otros sistemas, etc. Una línea por cada uno, explicando por qué queda fuera.

-
-
-

## 8. Detalles técnicos (si se hablaron)

| Campo | Valor |
|---|---|
| Tecnología o plataforma pedida | |
| Integraciones necesarias (pagos, correo, CRM, ERP…) | |
| ¿Necesita cuentas de terceros? ¿Quién las abre? | |
| ¿Hay datos que migrar de otro sistema? | |
| Requisitos legales o de facturación del país | |

## 9. Fases futuras (lo que queda para después)

> Lo que se conversó pero no entra en esta cotización. Sirve para que el cliente vea que no se
> olvidó, y para cotizarlo más adelante.

-
-

## 10. Cualquier otra cosa relevante

```
```

---

## Bloque opcional para acelerar la carga

> Si puedes, genera además este JSON con los mismos datos de arriba. Si no, no pasa nada: se
> arma a partir de las secciones anteriores.

```json
{
  "client": { "name": "", "company": "", "contact_name": "", "email": "", "phone": "" },
  "project_name": "",
  "summary": "",
  "hero_note": "",
  "currency": "USD",
  "phases": [
    {
      "name": "",
      "objective": "",
      "scope_limit": "",
      "timeline": "",
      "items": [
        { "title": "", "description": "", "cost": 0, "mandatory": "required", "time_estimate": "" }
      ]
    }
  ],
  "excluded": [""],
  "dependencies": [ { "title": "", "description": "", "responsible": "" } ]
}
```

`mandatory` solo acepta `"required"` u `"optional"`.

---

### Ejemplo de una fase bien rellenada

*(Para que se entienda el nivel de detalle esperado. Bórralo o déjalo, da igual.)*

### Fase 3 — Carrito y checkout con pasarela de pago

- **Qué se logra en esta fase:** El cliente agrega productos al carrito, completa sus datos de
  envío y paga con una pasarela real. Al confirmarse el pago, el pedido queda registrado y se
  envía confirmación por correo. La tienda queda lista para vender de verdad.
- **Qué NO entra en esta fase:** Una sola pasarela y una sola moneda. No incluye pagos en cuotas,
  cupones de descuento, cálculo de envío por peso ni facturación electrónica automática. La cuenta
  comercial con el proveedor de pagos la abre el cliente.
- **Cuándo se hace:** Días 5 a 8

| Entregable | Qué incluye | Costo | Obligatorio / Opcional | Tiempo estimado |
|---|---|---|---|---|
| Carrito de compras | Agregar, quitar y modificar cantidades, con cálculo automático de totales | 150 | Obligatorio | 9 a 11 horas |
| Flujo de checkout | Datos de envío, resumen del pedido y confirmación, con validación de cada campo | 180 | Obligatorio | 10 a 12 horas |
| Integración de la pasarela | Conexión segura en producción, manejo de credenciales y pruebas de pagos reales | 320 | Obligatorio | 14 a 16 horas |
| Correo de confirmación | Al aprobarse el pago se crea el pedido, se descuenta stock y se avisa al comprador | 100 | Obligatorio | 6 a 7 horas |
