# INVENTARIO · Venejugos C.A.

Dashboard del inventario de la bodega principal: **Módulo B**, **MobilVendor**, sus **diferencias** y la **carga de inventario manual** (conteo físico).
Se publica con GitHub Pages desde este repositorio (`Venejugosserver/INVENTARIO`, rama `main`).

| Página | Enlace | Para qué |
|---|---|---|
| INVENTARIO | `https://venejugosserver.github.io/INVENTARIO/inventario.html` | El dashboard |
| Panel administrativo | `https://venejugosserver.github.io/INVENTARIO/admin.html` | Carga de los exportes de Módulo B y MobilVendor y publicación |

`reporte.html` ya no forma parte del sistema y puede borrarse (el informe se descarga en Word desde el dashboard). También pueden borrarse `MOBILVENDOR.xlsx`, `MODULO_B.XLS` y `VENTAS.csv`.

## Dashboard INVENTARIO

- **Ver en el cuadro (slicer):** Módulo B · MobilVendor · Diferencias · Carga de inventario. Se pueden marcar varios. Cada pestaña propone su selección y luego se puede cambiar.
- **Buscar en todo el cuadro:** busca en todas las columnas (módulo, código, artículo, unidad de negocio, detalle, cantidad). Con varias palabras, deben aparecer todas.
- **Cuadro de inventario:** la primera columna, **Módulo**, indica de qué sistema viene cada cantidad. Se ordena pulsando los encabezados y se exporta a CSV.
- **Pestañas:** 1 · Módulo B · 2 · MobilVendor · 3 · Auditoría / Diferencias · 4 · Resumen escrito. Las pestañas 1 y 2 muestran de dónde viene la información (archivo, fecha, corte y columnas usadas).
- **Guardar informe en Word:** origen de la información, resumen escrito, auditoría por unidad de negocio, Nestlé y PepsiCo en sus unidades y el detalle de cada artículo con diferencia o con conteo.
- **Carga de inventario (apartado final):** se escribe el conteo físico de cada artículo y se pulsa **Guardar cambios**. Pide la clave, pregunta *¿Estás seguro de hacer este cambio?* y luego se pulsa **Publicar**, que escribe `carga_manual.json` en el repositorio.

La primera vez que se publica la carga en un navegador se pide un token *fine-grained* de GitHub con permiso *Contents: Read and write* sobre este repositorio (el mismo del panel administrativo). Queda guardado solo en ese navegador.

## Archivos del repositorio

| Archivo | Lo genera | Contenido |
|---|---|---|
| `inventario.html`, `admin.html` | Desarrollo | Páginas autocontenidas |
| `inventario.json` | Panel → Publicar inventario | Módulo B y MobilVendor cruzados por artículo |
| `carga_manual.json` | Dashboard → Guardar cambios | Conteo físico por artículo, con fecha y responsable |
| `README.md` | Desarrollo | Este documento |

Publicar un inventario nuevo desde el panel **no borra** la carga manual: son archivos distintos.

## Reglas

- Equivalencias: Artículo = alterno · Artículo (Nombre) = producto · Inventario = stock.
- Cruce: código exacto, luego código sin ceros a la izquierda, luego nombre idéntico.
- Unidad de negocio: la de Módulo B (sin pareja: la de su marca).
- Diferencias: MobilVendor − Módulo B; Carga − MobilVendor; Carga − Módulo B. Nestlé y PepsiCo se miden distinto en cada sistema: sus cantidades no se restan entre sistemas, solo el valor.
- El conteo manual se escribe en la unidad de MobilVendor (columna Medida).
- Valor MobilVendor = Monto; valor Módulo B = stock × costo_dolar (parece incluir IVA).

## Seguridad

La clave del dashboard evita cambios por error, pero la página es pública: la protección real para escribir en el repositorio es el token de GitHub, que solo tienen los navegadores autorizados. `inventario.json` y `carga_manual.json` pueden ser vistos por quien tenga el enlace.
