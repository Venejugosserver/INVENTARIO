# INVENTARIO · Venejugos C.A.

Dashboard del inventario de la bodega principal. **Inventario del sistema = MobilVendor + Módulo B**; las **diferencias** son entre ese total y el **inventario manual** (conteo físico) que se registra en el propio dashboard.
Se publica con GitHub Pages desde este repositorio (`Venejugosserver/INVENTARIO`, rama `main`).

| Página | Enlace | Para qué |
|---|---|---|
| INVENTARIO | `https://venejugosserver.github.io/INVENTARIO/inventario.html` | El dashboard |
| Panel administrativo | `https://venejugosserver.github.io/INVENTARIO/admin.html` | Dos secciones independientes: **1 · Inventario** (paneles MOBILVENDOR y MODULO B, en la mañana) y **2 · Ventas** (panel VENTAS, en la tarde), cada una con su botón de publicar |

`reporte.html` ya no forma parte del sistema y puede borrarse (el informe se descarga en Word desde el dashboard). También pueden borrarse `MOBILVENDOR.xlsx`, `MODULO_B.XLS` y `VENTAS.csv`.

## Dashboard INVENTARIO

- **Horas de actualización:** arriba se ven por separado la hora del inventario (MOBILVENDOR + MODULO B, mañana) y la de las ventas (tarde), y si el orden es el esperado.
- **Ver en el cuadro (slicer):** Módulo B · MobilVendor · Diferencias · Carga de inventario · Ventas. Cada uno agrega su bloque de columnas al cuadro, uno al lado del otro.
- **Buscar en todo el cuadro:** busca en todas las columnas (módulo, código, artículo, unidad de negocio, detalle, cantidad). Con varias palabras, deben aparecer todas.
- **Cuadro de inventario:** un artículo por fila. Bloque **Cantidad**: Módulo B · MobilVendor · Auditada · Diferencia (auditada − Módulo B − MobilVendor). Bloque **Detalle**: Módulo B · MobilVendor · Auditada. La primera columna, **Módulo**, indica en qué sistema está el artículo. Se ordena pulsando los encabezados y se exporta a CSV.
- **Pestañas:** 1 · Vista general (MobilVendor + Módulo B) · 2 · Módulo B · 3 · MobilVendor · 4 · Auditoría / Diferencias · 5 · Inventario manual · 6 · Resumen escrito. La vista general muestra lado a lado de dónde viene la información de cada sistema y sus artículos y unidades por unidad de negocio.
- **Navegación:** el dashboard tiene el botón *Panel administrativo →* y el panel tiene *Ir a la vista general →*.
- **Guardar informe en Word:** solo cantidades (sin costos ni valores en $): origen de la información, resumen escrito, auditoría por unidad de negocio, Nestlé y PepsiCo, artículos con diferencia (cantidad y detalle de Módulo B, MobilVendor y auditada) e inventario por artículo.
- **5 · Inventario manual:** avance del conteo, coincidencias con cada sistema y la tabla donde se escribe el conteo físico de cada artículo. Lo escrito queda en el navegador hasta pulsar **Guardar cambios**. Pide la clave, pregunta *¿Estás seguro de hacer este cambio?* y luego se pulsa **Publicar**, que escribe `carga_manual.json` en el repositorio.

La primera vez que se publica la carga en un navegador se pide un token *fine-grained* de GitHub con permiso *Contents: Read and write* sobre este repositorio (el mismo del panel administrativo). Queda guardado solo en ese navegador.

## Doble auditoría

1. **Auditoría del sistema (directa):** cada módulo contra sus propias ventas (MobilVendor contra las ventas MOBILVENDOR, Módulo B contra las ventas B, según la columna MODULOS del archivo VENTAS; unidades = venta_neta, columna BB) y el total del sistema contra el total vendido. Estados: *inventario cubre las ventas*, *ventas superan el inventario*, *vendido sin inventario*. Se ve en la pestaña 4, en el cuadro (botón Ventas del slicer) y en el informe en Word.
2. **Auditoría manual (por descripción):** en la pestaña 5 cada artículo aparece con su descripción en MobilVendor y en Módulo B (ordenado por descripción), las unidades de cada módulo y las ventas; se escribe el conteo físico y la diferencia es conteo − (MobilVendor + Módulo B).

La descripción de Módulo B se guarda en `inventario.json` desde esta versión: vuelva a publicar el inventario desde el panel para que aparezca (mientras tanto se muestra la descripción de MobilVendor).

## Archivos del repositorio

| Archivo | Lo genera | Contenido |
|---|---|---|
| `inventario.html`, `admin.html` | Desarrollo | Páginas autocontenidas |
| `inventario.json` | Panel → 1 · Publicar inventario | MOBILVENDOR y MODULO B unidos por artículo, con hora de publicación |
| `ventas.json` | Panel → 2 · Publicar ventas | Del archivo VENTAS (hoja DINAMICA): venta_neta (columna BB) por artículo y por módulo (MOBILVENDOR por codprod, B por alterno), del período y del último día, con hora de publicación; sin clientes, vendedores ni direcciones |
| `carga_manual.json` | Dashboard → Guardar cambios | Conteo físico por artículo, con fecha y responsable |
| `README.md` | Desarrollo | Este documento |

Publicar un inventario nuevo desde el panel **no borra** la carga manual: son archivos distintos.

## Reglas

- Equivalencias: Artículo = alterno · Artículo (Nombre) = producto · Inventario = stock.
- Cruce: código exacto, luego código sin ceros a la izquierda, luego nombre idéntico.
- Unidad de negocio: la de Módulo B (sin pareja: la de su marca).
- **Diferencia = conteo manual − (MobilVendor + Módulo B)**, por artículo y en la unidad de MobilVendor. Faltante si el conteo es menor; sobrante si es mayor; cuadra dentro de la tolerancia del panel. Solo se mide en los artículos contados.
- Nestlé y PepsiCo: Módulo B los lleva en otra presentación; antes de sumar se convierten a unidades con el factor de unidades por presentación de cada artículo (se estima internamente con los datos de los dos sistemas). Los que no dan un factor claro se marcan "factor por confirmar" y se suman como 1.
- El conteo manual se escribe en la unidad de la columna Medida (la de MobilVendor; en artículos que solo están en Módulo B, su presentación).
- El dashboard, el panel y el informe en Word muestran solo cantidades: no muestran costos ni valores en $.

## Seguridad

La clave del dashboard evita cambios por error, pero la página es pública: la protección real para escribir en el repositorio es el token de GitHub, que solo tienen los navegadores autorizados. `inventario.json` y `carga_manual.json` pueden ser vistos por quien tenga el enlace.
