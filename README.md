# DataProject — Dashboard & Análisis de Datos · Global Superstore

Análisis de ventas y rentabilidad de **Global Superstore** (2011–2014) y construcción de un dashboard interactivo en Google Sheets, a partir de un dataset de 51.290 registros y 147 países.

## ▶️ Probar el dashboard

### 🔗 **[ABRIR EL DASHBOARD INTERACTIVO](https://docs.google.com/spreadsheets/d/1yg4cji4Oo1wwoO7fzAmXkjW6rfWrhGCqoebjR2QtyNI/edit?usp=sharing)**

> **El enlace está abierto con permiso de edición: se puede usar directamente, sin pedir acceso, sin iniciar sesión y sin hacer ninguna copia.**
>
> Ve a la pestaña **`DashBoard_SuperStore`** y cambia cualquiera de los cuatro filtros amarillos: **Mercado, Segmento, Categoría y Año**. Los tres indicadores y las seis gráficas se recalculan a la vez.
>
> Prueba rápida: pon **Mercado = `EU`** y verás las ventas totales pasar de 12,6 M $ a 2,9 M $, con todas las gráficas actualizándose.

Si se prefiere trabajar en local, en la raíz de este repositorio está **`Proyecto_Dashboard_SuperStore.xlsx`**, el libro completo con sus seis pestañas y todas las fórmulas: se abre en Excel o se sube a Google Sheets con `Archivo → Importar` y los filtros funcionan igual.

---

![Dashboard Global Superstore](dashboard/captura_dashboard.png)

---

## Contenido del repositorio

```
├── README.md                              Este archivo
├── Proyecto_Dashboard_SuperStore.xlsx     Libro completo y funcional (descargar para probarlo)
├── data/
│   ├── raw/
│   │   └── Global_Superstore.xls          Dataset original sin modificar
│   └── clean/
│       ├── Datos_Limpios.csv              Datos tras la limpieza
│       └── Datos_Limpios.xlsx             Los mismos datos en formato Excel
├── docs/
│   ├── Informe_Analisis.md                Informe explicativo del análisis
│   ├── Registro_Limpieza.csv              Trazabilidad de la limpieza
│   └── Registro_Limpieza.xlsx             La misma trazabilidad en formato Excel
└── dashboard/
    └── captura_dashboard.png              Vista general del panel
```

## El dataset

| | |
|---|---|
| Fuente | Global Superstore (dataset público) |
| Periodo | Enero 2011 – diciembre 2014 |
| Registros | 51.290 líneas de pedido |
| Alcance | 25.035 pedidos · 795 clientes · 147 países · 7 mercados |
| Tablas | Orders (pedidos), Returns (devoluciones), People (responsables) |

## Proceso

**1. Transformación y limpieza.** Se detectaron y trataron catorce incidencias, entre ellas: clientes con dos identificadores distintos (1.590 IDs para 795 clientes reales), 457 códigos de producto asociados a varios nombres, regiones ambiguas que mezclaban continentes, nombres de país no estándar que impedían renderizar el mapa, una errata entre tablas ("AMEA" / "EMEA") y un 80% de valores vacíos en el código postal. Cada decisión queda registrada con el número de filas afectadas y su justificación en `Registro_Limpieza`. El dataset conserva las 51.290 filas originales y no contiene nulos.

**2. Análisis descriptivo.** Estudio de la evolución temporal, el mix de producto, el reparto geográfico, el efecto de los descuentos, las devoluciones y la logística.

**3. Dashboard.** Panel interactivo con tres indicadores, seis visualizaciones y cuatro filtros encadenados, construido sobre tablas de agregación que se recalculan automáticamente.

## Estructura del libro de cálculo

| Pestaña | Contenido |
|---|---|
| `Datos_SuperStore_Brutos` | Datos originales, sin modificar |
| `Datos_SuperStore_Limpios` | Datos limpios con columnas de apoyo |
| `Registro_Limpieza` | Problema, filas afectadas, decisión y justificación |
| `Responsables` | Responsables regionales |
| `Tablas_Dashboard` | Tablas de agregación que alimentan las gráficas |
| `DashBoard_SuperStore` | Panel final |

## Indicadores principales

| KPI | Valor |
|---|---|
| Ventas totales | 12.642.502 $ |
| Beneficio total | 1.467.457 $ |
| Margen medio | 11,6% |
| Crecimiento de ventas 2011→2014 | +90% |

## Principales conclusiones

- **Los descuentos destruyen el margen.** Las ventas sin descuento rinden un 25,3%; las rebajadas pierden 303.238 $ en conjunto (−5,4%). El 43% de las líneas se vende con descuento y el beneficio de la compañía procede íntegramente del resto.
- **Furniture vende mucho y aporta poco**: 32,5% de las ventas y solo el 19,4% del beneficio. **Tables es la única subcategoría en pérdidas** (−64.083 $).
- **Estacionalidad marcada**: el último trimestre concentra el 34% de las ventas anuales.
- **Concentración geográfica**: 10 países reúnen el 63% de la facturación. EMEA obtiene la mitad de margen que el resto y Turquía, Nigeria y Países Bajos operan en pérdidas.
- **El segmento de cliente no diferencia**: los tres segmentos tienen márgenes prácticamente idénticos.

El desarrollo completo está en [`docs/Informe_Analisis.md`](docs/Informe_Analisis.md).

## Herramientas

Google Sheets (limpieza mediante funciones, tablas de agregación, gráficos y controles de filtro).
