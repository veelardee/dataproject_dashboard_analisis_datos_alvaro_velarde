# Informe de Análisis — Dashboard Global Superstore

**Proyecto:** DataProject — Dashboard & Análisis de Datos
**Dataset:** Global Superstore (2011–2014) · 51.290 registros · 147 países
**Herramienta:** Google Sheets
**Dashboard interactivo:** https://docs.google.com/spreadsheets/d/1yg4cji4Oo1wwoO7fzAmXkjW6rfWrhGCqoebjR2QtyNI/edit

---

## 1. Objetivo del análisis

Global Superstore es una empresa de distribución de material de oficina, mobiliario y tecnología con presencia mundial. El objetivo de este proyecto es transformar y limpiar sus datos de ventas, analizarlos descriptivamente y construir un dashboard interactivo que permita responder a tres preguntas de negocio:

1. **¿De dónde viene el beneficio y de dónde no?** Identificar qué categorías y productos venden mucho pero aportan poco margen.
2. **¿Cómo evoluciona el negocio en el tiempo?** Detectar tendencia de crecimiento y estacionalidad.
3. **¿Dónde está concentrada la actividad?** Analizar el reparto geográfico de ventas y rentabilidad.

## 2. Descripción del dataset

El conjunto de datos procede del dataset público *Global Superstore* y se compone de tres tablas:

| Tabla | Registros | Contenido |
|---|---|---|
| Orders | 51.290 | Una fila por línea de pedido: cliente, producto, geografía, importes y fechas |
| Returns | 1.173 | Pedidos devueltos |
| People | 13 | Responsable comercial de cada región |

Cubre el periodo **enero 2011 – diciembre 2014**, con 25.035 pedidos de 795 clientes en 147 países, repartidos en 7 mercados (APAC, EU, US, LATAM, EMEA, Africa y Canada), 3 categorías y 17 subcategorías de producto. Los importes están expresados en dólares.

## 3. Transformación y limpieza de los datos

El detalle completo, con filas afectadas y justificación de cada decisión, está en la pestaña **Registro_Limpieza** del archivo. Resumen de los problemas detectados y su tratamiento:

| # | Problema detectado | Filas afectadas | Decisión |
|---|---|---|---|
| 1 | Espacios en blanco al inicio/final de textos | 16 | Aplicar TRIM a las columnas de texto |
| 2 | Comprobación de filas duplicadas | 0 | Sin acción: no existen duplicados |
| 3 | `Postal Code` vacío (80,5% de las filas, todo lo no estadounidense) | 41.296 | Eliminar la columna |
| 4 | Cada cliente tiene dos identificadores distintos (ej. AB-10015 y AB-15): 1.590 IDs para 795 clientes | 10.002 | Unificar al ID de 5 dígitos |
| 5 | Regiones ambiguas: *Central*, *North* y *South* mezclan varios mercados | 22.547 | Crear columna `Region Detallada` = Mercado + Región |
| 6 | 457 códigos de producto asociados a dos o más nombres distintos | 4.392 | Marcar en columna de control y analizar por nombre de producto |
| 7 | La tabla Returns usa "United States" donde Orders usa "US" | 296 | Normalizar a "US" |
| 8 | Devoluciones en tabla separada; 37 pedidos repiten número en distintos mercados | 3.043 | Añadir columna `Devuelto` cruzando por pedido **y** mercado |
| 9 | La tabla People escribe "AMEA" en lugar de "EMEA" | 1 | Corregir la errata |
| 10 | Nombres de país no estándar (*Myanmar (Burma)*, *Swaziland*, *Macedonia*…) que el mapa no reconoce | 636 | Añadir columna `Codigo Pais ISO` (ISO 3166-1 alfa-2) |
| 11 | Comprobación de coherencia de fechas (envío anterior al pedido) | 0 | Sin acción: todas coherentes |
| 12 | Faltan columnas de apoyo para el análisis | — | Crear Año, Mes, Año-Mes, Días Envío, Margen % y Con Pérdidas |
| 13 | Valores extremos en beneficio, de −6.600 $ a +8.400 $ (criterio IQR × 3) | 5.920 | Mantener: son operaciones reales, no errores |
| 14 | Cabeceras en inglés y sin orden lógico | — | Traducir al español y ordenar por fecha de pedido |

Tras la limpieza el conjunto conserva las **51.290 filas originales** y no contiene ningún valor nulo. Dos aclaraciones sobre decisiones que podrían discutirse:

- **Los outliers se mantienen.** El pedido con mayor pérdida (−6.600 $) corresponde a una impresora 3D vendida con un 70% de descuento: es una operación real y eliminarla falsearía el beneficio total.
- **Las líneas con beneficio negativo no son errores.** Son ventas que pierden dinero y constituyen precisamente uno de los hallazgos del análisis.

## 4. Análisis descriptivo

### 4.1 Indicadores generales

| KPI | Valor |
|---|---|
| Ventas totales | 12.642.502 $ |
| Beneficio total | 1.467.457 $ |
| Margen medio | 11,6% |
| Pedidos | 25.035 |
| Clientes | 795 |
| Plazo medio de envío | 4,0 días |

### 4.2 Evolución temporal

El negocio crece de forma sostenida durante los cuatro años, sin ningún ejercicio de caída:

| Año | Ventas | Beneficio | Pedidos |
|---|---|---|---|
| 2011 | 2.259.451 $ | 248.941 $ | 4.440 |
| 2012 | 2.677.439 $ | 307.415 $ | 5.343 |
| 2013 | 3.405.746 $ | 406.935 $ | 6.721 |
| 2014 | 4.299.866 $ | 504.166 $ | 8.531 |

Las ventas crecen un **90% entre 2011 y 2014** y el beneficio un 103%, es decir, la empresa no solo vende más sino que mejora ligeramente su rentabilidad.

La serie mensual muestra una **estacionalidad muy marcada**: el último trimestre concentra el 34% de las ventas anuales, con noviembre (1,55 M $) y diciembre (1,58 M $) como meses punta, frente a un mínimo claro en febrero (0,54 M $). El patrón se repite los cuatro años, lo que permite anticiparlo en planificación de stock y personal.

### 4.3 Categorías y productos

| Categoría | % Ventas | % Beneficio | Margen |
|---|---|---|---|
| Technology | 37,5% | 45,2% | 14,0% |
| Office Supplies | 30,0% | 35,3% | 13,7% |
| Furniture | 32,5% | 19,4% | 6,9% |

Aquí aparece el **principal desequilibrio del negocio**: Furniture representa casi un tercio de la facturación pero menos de una quinta parte del beneficio, con un margen (6,9%) que es la mitad del de las otras dos categorías.

Bajando a subcategoría se identifica al responsable: **Tables es la única subcategoría que pierde dinero**, con 757.041 $ de ventas y −64.083 $ de beneficio (margen −8,5%). Le siguen en baja rentabilidad Machines (7,6%) y Chairs (9,3%). En el extremo opuesto, **Copiers** es el producto más rentable en términos absolutos (258.568 $ de beneficio, margen 17,1%) y **Paper** el de mayor margen relativo (24,2%).

### 4.4 Análisis geográfico

| Mercado | Ventas | Beneficio | Margen |
|---|---|---|---|
| APAC | 3.585.744 $ | 436.000 $ | 12,2% |
| EU | 2.938.089 $ | 372.830 $ | 12,7% |
| US | 2.297.201 $ | 286.397 $ | 12,5% |
| LATAM | 2.164.605 $ | 221.643 $ | 10,2% |
| EMEA | 806.161 $ | 43.898 $ | 5,4% |
| Africa | 783.773 $ | 88.872 $ | 11,3% |
| Canada | 66.928 $ | 17.817 $ | 26,6% |

Estados Unidos es el mayor mercado individual (2,3 M $), seguido de Australia, Francia y China. Los **diez primeros países concentran el 63% de las ventas** de los 147 totales.

Por rentabilidad el panorama cambia: **EMEA obtiene un margen del 5,4%**, menos de la mitad que el resto, y hay países que operan directamente en pérdidas: Turquía (−98.447 $), Nigeria (−80.751 $) y Países Bajos (−41.070 $). Canadá, en cambio, es el mercado más rentable con un 26,6% de margen, aunque su volumen es marginal.

### 4.5 El efecto de los descuentos

Este es el hallazgo más accionable del análisis. Separando las ventas con y sin descuento:

| | Ventas | Beneficio | Margen |
|---|---|---|---|
| Sin descuento | 6.992.411 $ | 1.770.695 $ | **25,3%** |
| Con descuento | 5.650.091 $ | **−303.238 $** | **−5,4%** |

**El 43% de las líneas se vende con algún descuento, y ese bloque pierde dinero en conjunto.** El beneficio de la compañía procede íntegramente de las ventas sin rebaja. El punto de inflexión está alrededor del 25%: hasta ahí el margen se reduce pero sigue siendo positivo (16,4% con un 10% de descuento, 9,8% con un 20%); a partir del 30% el margen se vuelve negativo y ya no se recupera.

En conjunto, **12.544 líneas de pedido (el 24,5% del total) se cierran con pérdidas**, acumulando −920.646 $.

### 4.6 Devoluciones y logística

Las devoluciones afectan a 3.043 líneas (5,9% del total) por valor de 818.044 $, una tasa contenida.

El **Standard Class** concentra el 60% de los envíos con un plazo medio de 5,0 días, frente a los 2,2 días de First Class. No se observan diferencias relevantes de margen entre modalidades, por lo que el modo de envío no explica la pérdida de rentabilidad.

### 4.7 Segmento de cliente

El segmento Consumer aporta el 51,5% de las ventas, Corporate el 30,3% y Home Office el 18,3%, con márgenes prácticamente idénticos entre sí (11,5% – 12,0%). El tipo de cliente, por tanto, **no es una palanca de rentabilidad**: la diferencia está en el producto, la política de descuentos y el mercado.

## 5. Conclusiones

1. **El negocio crece de forma sana**: +90% de ventas en cuatro años con mejora del beneficio, y una estacionalidad de cuarto trimestre predecible y aprovechable.
2. **La política de descuentos destruye el margen.** Las ventas rebajadas pierden 303.238 $ en conjunto mientras que las no rebajadas rinden un 25,3%. Es la palanca con mayor impacto potencial: limitar los descuentos por encima del 25% convertiría en positivo un bloque de negocio que hoy resta.
3. **Furniture, y en concreto Tables, lastra la rentabilidad.** Las mesas son la única subcategoría en pérdidas. Conviene revisar su precio de coste, su política de descuentos o su permanencia en catálogo.
4. **Hay mercados que no compensan.** EMEA obtiene la mitad de margen que el resto, y Turquía, Nigeria y Países Bajos operan en pérdidas. Merecen una revisión específica de costes de envío y descuentos aplicados.
5. **El crecimiento no depende del tipo de cliente**, sino del mix de producto y de la disciplina comercial.

## 6. Metodología y herramientas

Todo el proyecto se ha desarrollado en **Google Sheets**. El libro se organiza en seis pestañas:

- `Datos_SuperStore_Brutos` — datos originales sin modificar, para comparación.
- `Datos_SuperStore_Limpios` — datos tras las 14 operaciones de limpieza, con columnas de apoyo añadidas.
- `Registro_Limpieza` — trazabilidad de cada decisión de transformación.
- `Responsables` — tabla de responsables regionales corregida.
- `Tablas_Dashboard` — tablas de agregación que alimentan las visualizaciones.
- `DashBoard_SuperStore` — panel final.

El dashboard es **interactivo**: dispone de cuatro filtros (Mercado, Segmento, Categoría y Año) que recalculan simultáneamente los tres indicadores y las seis visualizaciones, lo que permite analizar cualquier combinación de mercado, periodo y línea de producto sin modificar los datos de origen.
