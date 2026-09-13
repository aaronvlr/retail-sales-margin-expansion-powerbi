# Executive Sales & Margin Analysis Dashboard (Power BI)

<img width="1318" height="744" alt="dashboard_preview" src="https://github.com/user-attachments/assets/9a4bd2c1-09e4-4278-8897-89bd36e75772" />

## 1. Contexto del Negocio y Problema
Una compañía minorista internacional con operaciones en Norteamérica experimentó un aumento continuo en su volumen de facturación bruta; no obstante, el comité ejecutivo detectó un estancamiento severo en el margen neto consolidado. 

El objetivo de este proyecto fue diseñar un modelo analítico integral en Power BI para auditar las causas raíz de la fuga de capital, evaluar la dispersión de descuentos por subcategoría y ofrecer visibilidad transaccional a nivel directivo.

---

## 2. Principales Hallazgos Analíticos (Key Insights)
* **Fuga de margen por sobre-descuento:** Subcategorías con alta facturación como *Tables*, *Bookcases* y *Supplies* registran márgenes netos negativos. Al auditar las órdenes individuales, se detectó que el equipo de ventas aplicó de forma recurrente descuentos de entre el 50% y el 80%, absorbiendo cualquier ganancia operativa.
* **Vulnerabilidad en la Región Central:** Mientras que la región *West* mantiene márgenes saludables superiores a los $20 mil USD, la región *Central* presenta la menor rentabilidad histórica debido a una alta concentración de pedidos institucionales con políticas de precio agresivas.
* **Líneas protectoras del negocio:** Subcategorías como *Copiers*, *Accessories* y *Phones* sostienen la viabilidad operativa con márgenes superiores al 20%, demandando menor elasticidad promocional para cerrar acuerdos.

---

## 3. Recomendaciones Estratégicas
1. **Límites estrictos a la política de descuentos:** Implementar un tope máximo de descuento del 20% para la categoría de Mobiliario (*Furniture*), requiriendo aprobación de la dirección financiera para márgenes menores al 10%.
2. **Revisión de acuerdos comerciales en Región Central:** Auditar los contratos de clientes corporativos en la región central para renegociar tarifas y eliminar subsidios en costos de envío.
3. **Rediseño de compensaciones para el equipo comercial:** Alinear las comisiones de los ejecutivos de cuenta al **Margen Neto Generado** en lugar del volumen bruto de facturación, desincentivando el remate de inventario con sobre-descuentos.

---

## 4. Arquitectura Técnica y Modelado de Datos

### Modelo Dimensional (Esquema Estrella)
Siguiendo las mejores prácticas de arquitectura de Ralph Kimball, se implementó un esquema estrella estricto de relaciones unidireccionales de **uno a varios (1:*)** conectadas a una tabla transaccional de hechos:
* `Fact_Ventas`: Registro transaccional de ventas, unidades, descuentos y margen neto.
* `Dim_Cliente`: Segmentación de compradores (`Customer ID`, `Customer Name`, `Segment`).
* `Dim_Producto`: Jerarquía de productos (`Product ID`, `Category`, `Sub-Category`, `Product Name`).
* `Dim_Geografia`: Normalización territorial (`Postal Code`, `City`, `State`, `Region`).
* `Dim_Calendario`: Tabla dimensional continua generada en DAX, marcada como *Date Table* para Time Intelligence.

### Medidas DAX Principales
Todos los cálculos fueron centralizados en una tabla contenedora independiente `_Medidas`:
* **Margen Neto Pct:** `DIVIDE([Total Ganancia], [Total Ventas], 0)`
* **Ventas Año Anterior (SPLY):** `CALCULATE([Total Ventas], SAMEPERIODLASTYEAR(Dim_Calendario[Date]))`
* **Crecimiento Interanual (YoY):** `DIVIDE([Total Ventas] - [Ventas SPLY], [Ventas SPLY], 0)`
* **Título Dinámico:** Evaluación contextual en DAX del crecimiento o contracción del período seleccionado para alimentar dinámicamente los encabezados visuales.

---

## 5. Cómo explorar este proyecto
1. Descarga el archivo `Retail_Sales_Executive_Dashboard.pbix` de este repositorio.
2. Ábrelo con [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
3. Interactúa con los filtros anuales y la selección cruzada por subcategoría para auditar las órdenes afectadas.
