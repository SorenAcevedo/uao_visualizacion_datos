# Diccionario de datos — Proyecto de Visualización

**Facultad:** Facultad de Ingeniería y Ciencias Básicas

**Programa:** Maestría en Inteligencia Artificial y Ciencia de Datos

**Curso:** Visualización de Datos

**Estudiantes:**
- Soren Fabricius Acevedo (22500566)
- Ricardo Muñoz Bocanegra (22500246)
- Nicolas Lozano Mazuera (22500565)
- Juan José Bonilla Pinzón (22502052)
- Juan Manuel García Ortiz (22502268)

Objetivo: definir las variables a utilizar, justificar selección, y detallar tipo, unidad, rango, periodicidad y agregaciones recomendadas.

**Fuente de datos:** [Tasas de interés activas por tipo de crédito – Últimos dos meses](https://www.datos.gov.co/Econom-a-y-Finanzas/Tasas-de-inter-s-activas-por-tipo-de-cr-dito-ltimo/qzsc-9esp/about_data)

**Selección**: Incluidas `Fecha_Corte`, `Nombre_Entidad`, `Producto_Credito`, `Tasa_Interes_Act`, `Montos_Desemb`, `Codigo_Mun`, `Sexo`. `Nombre_Tipo_Entidad`.

| Variable | Descripción | Tipo | Unidad | Dominio / Rango | Periodicidad | Agregaciones | Justificación |
|---|---|---:|---|---|---|---|---|
| Fecha_Corte | Fecha de referencia del reporte | Temporal | N/A (ISO YYYY-MM-DD) | Fechas válidas dentro del periodo del dataset (ej. 2018-01-01 — 2026-12-31) | Semanal | Trimestre, Año, ventanas móviles, resúmenes (sumas, promedios) | Permite análisis temporal y series de tendencia |
| Nombre_Entidad | Nombre del banco o entidad | Categórica | N/A | Catálogo SFC (lista cerrada). Normalizar nombres y agrupar en "Otros" si procede | Semanal | Conteos, participación %, top-k, agrupaciones | Identifica responsable de colocación; clave para comparaciones entre entidades |
| Nombre_Tipo_Entidad | Tipo de compañía (banco, cooperativa, fintech) | Categórica | N/A | Lista cerrada / catálogo interno; validar y normalizar | Semanal | Grupos, conteos, participación % | Permite segmentación por tipo organizacional; útil para comparaciones estructurales |
| Producto_Credito | Tipo de cartera (vivienda, consumo, etc.) | Categórica | N/A | Lista cerrada de productos; validar y normalizar | Semanal | Filtros por producto, sumas y conteos por producto | Segmentación por tipo de crédito para análisis de cartera |
| Tasa_Interes_Act | Tasa de interés informada | Numérica | % E.A. | 0% — 100% (verificar outliers: negativos o >100%) | Semanal | Promedio ponderado (por monto), mediana, percentiles | Métrica financiera central; usar ponderación por monto para promedios más representativos |
| Montos_Desemb | Monto total colocado (desembolsado) | Numérica | COP | >= 0; revisar outliers y máximos plausibles | Semanal | Sumatoria (total), promedio por operación, crecimiento interperiodo, participación % | Base para magnitud económica y participación de mercado |
| Codigo_Mun | Código DANE del municipio | Categórica | N/A | Código DANE de 5 dígitos; valores inválidos → missing | Semanal | Agregación a nivel departamento, mapas, sumas por municipio | Permite análisis geográfico y mapeo de flujos |
| Sexo | Género del titular | Categórica | N/A | `M`, `F`, `N/A` | Semanal | % participación por sexo, segmentación de montos/tasas | Soporta análisis sociodemográficos y equidad |


## Variables No Utilizadas y Justificación

Teniendo en cuenta que el dataset cuenta con 23 variables y para cumplir los objetivos del análisis o Dashboard, hemos seleccionado las principales variables (8). A continuación justificamos la exclusión de las otras 15 variables.

- **Redundancia Operativa** (`Tipo_Entidad`, `Codigo_Entidad`, `Numero_de_creditos_desembolsados`): Para el análisis de competencia, el nombre comercial de la entidad es suficiente; estos campos añaden duplicidad operativa.

- **Fuera del Alcance Estratégico** (`Tipo_de_Persona`, `Tamaño_de_Empresa`, `Clase_deudor`, `Codigo_CIIU`, `Tipo_de_crédito`, `Grupo_Etnico`, `Antigüedad_de_la_empresa`, `Tipo_de_garantía`): Corresponden a micro-segmentaciones operativas. Nuestro enfoque está en la planeación comercial macro y regional; incluir estas dimensiones añadiría complejidad innecesaria que no responde a la pregunta de negocio planteada.

- **Complejidad y Ruido Visual** (`Plazo`, `Margen_adicional`, `Tipo_de_Tasa`, `Rango_monto_desembolsado`): El factor decisor en la estrategia de pricing será la Tasa Promedio Ponderada. Componentes internos de la tasa o detalles de plazos son tácticos y no aportan valor a la visualización de tendencia de mercado y volumen de desembolsos.

Estas variables pueden conservarse en el dataset para análisis operativos o futuros exploratorios, pero quedan excluidas del conjunto principal utilizado para el dashboard por las razones expuestas.
