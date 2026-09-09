# Portal de proyectos y cotizaciones

Plataforma de una sola página (sin build) para presentar proyectos a clientes: cada cliente entra
con el **código y la clave de su proyecto** y ve su cotización desglosada, las fases con su avance,
los documentos descargables y un espacio para dejar notas. Desde el panel de administración se ven
todos los proyectos, su historial y se actualiza el avance.

**Sitio publicado:** https://rengifojjrr.github.io/cotizaciones-interactivas/

| Página | Para quién | Qué hace |
|---|---|---|
| `portal.html` | Clientes | Entra con código + clave y navega por botones: cotizaciones, fases, documentos y notas. |
| `admin.html` | Equipo interno | CRM de clientes con sus claves de acceso, proyectos, avance e historial. |
| `index.html` | Clientes (cotizador) | Cotizador interactivo de alcance por módulos (proyecto "Ecosistema web integral"). |
| `monpica.html` | Clientes (cotizador) | Cotizador interactivo por fases del proyecto MONPICA-SGP. Sin horas: el precio es cerrado. |

## Tema claro y oscuro

Las tres páginas tienen un botón de tema (🌗) junto al logo que cicla entre **automático**
(sigue la configuración del teléfono o del sistema), **claro** y **oscuro**. La elección se guarda
en el navegador de cada persona, así que el cliente puede verlo oscuro aunque tú lo uses claro.

El documento de cotización que se descarga o imprime se mantiene siempre en claro, como debe ser
para un PDF que se envía o se imprime.

## Cómo entra el cliente

El portal abre en un menú de botones grandes — **Cotizaciones**, **Fases y avance**, **Documentos**
y **Notas y preguntas** — cada uno con su contador en vivo. Los botones sin contenido no aparecen.

Hay dos formas de acceso, y ambas funcionan:

- **Por cliente (recomendado):** el cliente entra con su propio código y clave, y ve *todo lo suyo*
  en un mismo espacio: todas sus cotizaciones (las de proyectos y las interactivas), el avance de
  cada proyecto, los documentos y sus notas. Se crea desde la ficha del cliente en el CRM.
- **Por proyecto:** la clave de un proyecto abre solo ese proyecto. Sirve para compartir una sola
  cosa, y mantiene vivos los links entregados antes de que existiera el acceso por cliente.

## Plantilla para levantar un proyecto nuevo

`PLANTILLA-PROYECTO.md` es un formulario en blanco para pasárselo al chat donde se está
conversando el proyecto. Ese chat lo rellena con el cliente, el alcance, las fases, los costos y
los tiempos, y el archivo devuelto tiene todo lo necesario para armar la cotización, cargar el
proyecto en el portal y generar los documentos por fase.

## Cómo compartir un proyecto con un cliente

1. Entra a `admin.html` con la clave de administrador.
2. **+ Nuevo proyecto** y pega el JSON del proyecto (hay una plantilla cargada por defecto).
   `code` y `access_key` son obligatorios en un proyecto nuevo.
3. Clic en **Copiar link** y envíale al cliente ese enlace junto con su clave.
   El link ya lleva el código puesto (`portal.html?p=CODIGO`), el cliente solo escribe la clave.

Cada proyecto tiene su propia clave: una clave no abre el proyecto de otro cliente.

## CRM de clientes

`admin.html` abre en la pestaña **Clientes (CRM)**. Cada cliente tiene una ficha con:

- Sus datos de contacto (nombre, empresa, persona de contacto, correo, teléfono) y notas internas
  que el cliente nunca ve.
- Sus proyectos, cada uno con un bloque **Datos de acceso del cliente**: código, clave (oculta,
  con botón *Ver*), link del portal y un botón **Copiar todo para enviar** que arma el mensaje
  completo listo para pegarle al cliente por WhatsApp o correo.
- **Cambiar** la clave cuando haga falta: la anterior deja de servir al instante.
- Sus cotizaciones interactivas y todo su historial de actividad.
- **Acceso del cliente a su portal**: su código y clave propios (los que abren todo lo suyo), con
  los mismos botones de ver, copiar, cambiar y "copiar todo para enviar".

En la pestaña *Cotizaciones interactivas* cada cotización tiene dos selectores: a qué cliente
pertenece y si el cliente la ve o queda oculta. Con el botón *Cambiar cómo lo ve el cliente* se le
pone el nombre con el que aparece en su portal (por ejemplo, "Cotización — Ecosistema web integral"),
porque el nombre interno suele quedar vacío.

Un cliente puede tener varios proyectos y varias cotizaciones. Desde la pestaña *Cotizaciones
interactivas* se asigna cada cotización a un cliente con el selector de la derecha.

### Sobre las claves de acceso

La clave de cada proyecto se guarda de dos formas: **hasheada** (para validar el acceso) y
**cifrada** (para poder mostrártela si el cliente la pierde). El cifrado solo se abre desde una
función que exige tu clave de administrador, así que un volcado de la base de datos no la revela.
Con la clave pública de la web no se puede leer ninguna tabla del CRM ni recuperar ninguna clave.

## Cómo actualizar el avance

En **Ver detalle** de cada proyecto hay un selector de estado por fase y por entregable:

- Marcar una fase como **completada** completa automáticamente sus entregables.
- Marcar entregables sueltos mueve la fase sola a *en curso* o *completada* según corresponda.
- Todo queda registrado en el historial del proyecto, junto con lo que hace el cliente en su portal.

## Formato del JSON de un proyecto

```json
{
  "code": "TIENDA-F1",
  "access_key": "CLAVE-DEL-CLIENTE",
  "client_name": "Nombre del cliente",
  "project_name": "Nombre del proyecto",
  "summary": "Explicación breve, en lenguaje cercano.",
  "status": "propuesta",
  "hero_note": "Entrega estimada: 2 semanas.",
  "phases": [
    {
      "name": "Fase 1 — Nombre",
      "objective": "Qué se logra.",
      "scope_limit": "Lo que no entra.",
      "status": "pendiente",
      "timeline": "Días 1 a 5",
      "items": [
        {"title": "Entregable", "description": "Qué incluye", "cost": 100,
         "mandatory": "required", "time_estimate": "4 a 6 horas", "status": "pendiente"}
      ]
    }
  ],
  "documents": [
    {"name": "Propuesta en PDF", "description": "Desglose completo", "url": "https://…", "kind": "pdf"}
  ]
}
```

- `status` del proyecto: `propuesta`, `aprobado`, `en_desarrollo`, `entregado`, `pausado`.
- `status` de fase: `pendiente`, `en_curso`, `completada`. De entregable: `pendiente`, `en_curso`, `completado`.
- `mandatory`: `required` (entra en el alcance base) u `optional` (complemento que el cliente decide).
- Al editar un proyecto existente, si dejas `access_key` vacío la clave del cliente **no cambia**.
- Si mandas `phases` o `documents`, se reemplazan por completo; si los omites, se quedan como están.

## Backend (Supabase)

Proyecto `cotizaciones-interactivas`. Tablas: `cq_clients`, `cq_projects`, `cq_phases`, `cq_items`,
`cq_documents`, `cq_events`.

Todas las tablas tienen RLS activo y **sin políticas públicas**: con la clave pública no se puede
leer ni listar nada directamente. Todo pasa por funciones `SECURITY DEFINER`:

| Función | Quién la usa | Qué exige |
|---|---|---|
| `cq_portal_open(code, key)` | Cliente | Código + clave correctos de ese proyecto |
| `cq_portal_save_state(code, key, state)` | Cliente | Lo mismo; solo guarda sus notas |
| `cq_portal_log(code, key, …)` | Cliente | Lo mismo; solo inserta en el historial |
| `cq_admin_list / get / events` | Equipo | Clave de administrador |
| `cq_admin_save_project(key, payload)` | Equipo | Clave de administrador |
| `cq_admin_set_status(key, …)` | Equipo | Clave de administrador |
| `cq_admin_delete_project(key, id)` | Equipo | Clave de administrador |
| `cq_admin_clients / client / save_client` | Equipo | Clave de administrador |
| `cq_admin_reveal_key(key, project_id)` | Equipo | Clave de administrador |
| `cq_admin_set_key(key, project_id, nueva)` | Equipo | Clave de administrador |
| `cq_admin_link_quote(key, quote_id, client_id)` | Equipo | Clave de administrador |

La clave de administrador se guarda **hasheada** (SHA-256). La de cada cliente se guarda hasheada
para validar el acceso y además cifrada, para poder reenviársela desde el CRM. Ninguna clave está
en este repositorio ni en texto plano en la base de datos.

## Cotizadores interactivos

Hay dos, con el mismo motor y datos distintos. El cliente arrastra entregables entre el bloque al
que pertenecen, **Para después** y **No me interesa**, y ve el total moverse en vivo. La casilla de
*solo lo obligatorio* muestra qué queda si se deja únicamente lo esencial, sin mover nada.

`monpica.html` cotiza MONPICA-SGP con tres niveles, igual que el de CCR: **fase → entregable →
subtarea**. Son 9 fases, 29 entregables y 200 subtareas, cada una con su rango de horas y su parte
del precio. Las fases del bloque **comprometido** (levantamiento y fases 0 a 3) llevan precio firme;
las **referenciales** (4 a 7) llevan precio estimado y sin fecha, y se distinguen en el nombre de la
etapa. La suma tiene que dar 66.250 comprometido, 72.000 referencial y 138.250 en total: si se
edita el desglose, hay que volver a cuadrarla.

## Cotizador interactivo (`index.html`)

Se mantiene el cotizador de alcance por módulos del proyecto "Ecosistema web integral" (13 módulos,
USD 13,000), con drag & drop entre módulos, vista previa de "solo lo obligatorio", generación de
cotización formal e historial. Sus cotizaciones siguen visibles en la parte baja de `admin.html`.

## Notas

- Los valores no incluyen hosting, dominio, comisiones de pasarelas de pago, licencias ni servicios de terceros.
- El portal es de solo lectura para el cliente: puede ver y dejar notas, no puede cambiar montos ni estados.
