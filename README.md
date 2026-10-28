# Inventario · Venejugos C.A.

Conciliación del inventario de la bodega principal entre **MobilVendor** y **Módulo B**.
Se publica con GitHub Pages desde este repositorio (`Venejugosserver/INVENTARIO`, rama `main`).

| Página | Enlace | Para qué |
|---|---|---|
| Tablero | `https://venejugosserver.github.io/INVENTARIO/inventario.html` | Exactitud, brecha en valor, unidades de negocio, Nestlé, PepsiCo y detalle por artículo |
| Reporte por unidad | `https://venejugosserver.github.io/INVENTARIO/reporte.html` | Documento por unidad de negocio, descargable en Word o PDF |
| Panel administrativo | `https://venejugosserver.github.io/INVENTARIO/admin.html` | Carga de exportes, parámetros y publicación |

No hay `index.html`: comparta siempre los enlaces directos.

---

## Archivos del repositorio

| Archivo | Lo genera | Contenido |
|---|---|---|
| `inventario.html`, `reporte.html`, `admin.html` | Desarrollo | Páginas autocontenidas (HTML, CSS y JavaScript en un solo archivo) |
| `inventario.json` | Panel → Publicar inventario | Conciliación por artículo: códigos, nombres, existencias, costos y venta total por artículo |
| `README.md` | Desarrollo | Este documento |

`MOBILVENDOR.xlsx`, `MODULO_B.XLS` y `VENTAS.csv` (versión anterior) ya no se usan y pueden borrarse.
Para actualizar páginas, suba solo los `.html`: GitHub reemplaza los que tienen el mismo nombre.

---

## Flujo de actualización

1. Abra el **panel administrativo** y arrastre juntos los exportes: inventario de MobilVendor (`.xlsx`), inventario de Módulo B (`.xls`) y, para la cobertura, el reporte general de ventas (`.xlsx`). Cada archivo se reconoce por su contenido; el nombre no importa.
2. Revise *Data general* y la vista previa.
3. Pulse **Publicar inventario**: escribe `inventario.json` en el repositorio. Sin token, use *Descargar inventario.json* y súbalo a mano.
4. Espere uno o dos minutos y recargue el tablero (Ctrl+F5).

Las páginas leen primero `inventario.json` publicado; si responde 404 lo leen directo del repositorio, después la copia guardada en el navegador y, como último recurso, la copia incluida en el archivo.

---

## Reglas de medición

- **Equivalencias:** Artículo = alterno · Artículo (Nombre) = producto · Inventario = stock.
- **Cruce:** código exacto; si no aparece, código sin ceros a la izquierda; si tampoco, nombre idéntico (sin acentos ni signos).
- **Unidad de negocio:** la de Módulo B. Un artículo sin pareja toma la unidad que Módulo B le asigna a su misma marca en MobilVendor.
- **Estados:** Cuadra (misma cantidad, dentro de la tolerancia) · Difiere · En cero en Módulo B · Solo MobilVendor · Solo Módulo B (solo artículos con stock).
- **Exactitud** = cuadran ÷ artículos presentes en los dos sistemas. Verde ≥ 95 %, ámbar ≥ 80 %, rojo < 80 %.
- **Nestlé y PepsiCo** se miden distinto en cada sistema: tienen pestaña propia, sus cantidades se muestran lado a lado con su propia escala y no se restan.
- **Valor:** MobilVendor = columna Monto; Módulo B = stock × costo_dolar (parece incluir IVA en muchos artículos). La brecha en dólares es aproximada.
- **Cobertura** = inventario de MobilVendor ÷ venta diaria neta (facturas menos notas de crédito) del período del reporte. Solo para las unidades que trae el reporte de ventas. Riesgo < 7 días, sobrestock > 60 días (editables en el panel).

---

## Notas técnicas

- Páginas estáticas, sin servidor. SheetJS va incluido en el panel.
- El panel publica con la API de contenidos de GitHub; necesita un token *fine-grained* con permiso *Contents: Read and write* sobre este repositorio, guardado solo en el navegador de quien publica.
- `inventario.json` no lleva clientes, vendedores, teléfonos ni direcciones del reporte de ventas, solo la venta total por artículo.
- El repositorio es público: `inventario.json` puede ser visto por quien tenga el enlace.
