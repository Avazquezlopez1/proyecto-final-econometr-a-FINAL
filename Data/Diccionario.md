# Diccionario de datos – Venta de vehículos ligeros en México (2016–2025)


## Descripción general

El archivo data.csv tiene 120 observaciones mensuales (enero 2016 – diciembre 2025) y 19 columnas: 3 de identificación, la variable dependiente con su desglose y 13 explicativas. No tiene valores faltantes.

| Elemento | Valor |
| --- | --- |
| Archivo | `dataset.csv` (separado por comas, punto decimal) |
| Unidad de análisis | Un mes, México |
| Periodo | Enero 2016 – diciembre 2025 |
| Observaciones | 120 |
| Variable dependiente | `ventas_vehiculos_total` |
| Variables explicativas | 13 |
| Fuentes | INEGI (RAIAVL, INPC, ENOE, IGAE) y Banco de México (SIE) |

## Variables

Cada fila es una columna del archivo, en el mismo orden. El signo esperado es la hipótesis sobre su asociación con las ventas, manteniendo lo demás constante.

| Variable | Descripción | Unidad | Fuente (clave) | Transformación | Signo esperado |
| --- | --- | --- | --- | --- | --- |
| `fecha` | Mes de la observación | AAAA-MM | — | — | Identificador |
| `anio` | Año | Año | — | — | Identificador |
| `mes` | Mes (1 = enero) | 1–12 | — | Base de las ficticias de mes | Identificador |
| `ventas_vehiculos_total` | Venta al público de vehículos ligeros | Unidades | INEGI, RAIAVL | Suma de todos los modelos del mes | Y (dependiente) |
| `ventas_automoviles` | Ventas de automóviles | Unidades | INEGI, RAIAVL | Suma por tipo de vehículo | Desglose de Y |
| `ventas_camiones_ligeros` | Ventas de camiones ligeros (SUV, pickup, minivan) | Unidades | INEGI, RAIAVL | Suma por tipo de vehículo | Desglose de Y |
| `inpc_var_anual` | Inflación general anual | % | Banxico SIE (SP30578) | Ninguna | − |
| `tasa_desocupacion` | Tasa de desocupación nacional | % de la PEA | INEGI BIE, ENOE | Serie desestacionalizada | − |
| `tasa_objetivo_promedio_mes` | Tasa objetivo de Banxico | % | Banxico SIE (SF61745) | Promedio mensual de la serie diaria | − |
| `PIB IGAE` | Indicador Global de la Actividad Económica | Índice 2018 = 100 | INEGI, IGAE | Serie desestacionalizada | + |
| `precio_real_automoviles` | Precio real de los automóviles | Índice | INEGI, INPC genérico 206 | Genérico ÷ INPC general × 100 | − |
| `precio_real_gasolina` | Precio real de la gasolina de bajo octanaje | Índice | INEGI, INPC genérico 214 | Genérico ÷ INPC general × 100 | − |
| `precio_real_seguro_auto` | Precio real del seguro de automóvil | Índice | INEGI, INPC genérico 271 | Genérico ÷ INPC general × 100 | − |
| `precio_real_tramites_vehiculares` | Precio real de placas, tenencia y refrendo | Índice | INEGI, INPC genérico 220 | Genérico ÷ INPC general × 100 | − |
| `precio_real_reparacion_auto` | Precio real de la reparación de automóvil | Índice | INEGI, INPC genérico 217 | Genérico ÷ INPC general × 100 | + (sustituto: reparar el auto viejo) |
| `precio_real_transporte_publico` | Precio real del transporte público | Índice | INEGI, INPC subíndice 6.1 | Índice ÷ INPC general × 100 | + (sustituto) |
| `remesas_millones_usd` | Ingresos por remesas familiares | Millones de dólares | Banxico SIE (SE27803) | Ninguna | + |
| `tasa_informalidad_til1` | Tasa de informalidad laboral 1 (TIL1), nacional | % de la población ocupada | INEGI BIE, ENOE (clave 444673) | Serie original (no hay desestacionalizada) | − |
| `credito_automotriz_real` | Saldo de crédito automotriz vigente de la banca comercial | Millones de pesos de jul. 2018 | Banxico SIE, cuadro CF835 (SF266105) | Saldo nominal ÷ INPC general × 100 | + |

## Transformaciones y limpieza

Todas las series se recortaron a enero 2016 – diciembre 2025, el periodo en que todas tienen datos completos. Cada transformación se hizo con código y se puede reproducir.

1. **Ventas de vehículos:** se sumaron por mes las ventas de todos los modelos. Las celdas con "-" se tomaron como 0. Se conservaron las cantidades negativas que reporta INEGI (devoluciones de periodos anteriores). 
2. **Tasa objetivo:** la serie diaria se promedió por mes, así que refleja los cambios de tasa a mitad de mes.
3. **Precios reales del INPC:** cada índice genérico se dividió entre el INPC general del mismo mes y se multiplicó por 100. Si sube, ese bien se encareció más que el promedio de la economía.
4. **Crédito automotriz real:** el saldo en millones de pesos nominales se dividió entre el INPC general y se multiplicó por 100.
5. **Variables que se crean en el notebook:** el logaritmo de las ventas, 11 ficticias de mes (enero como referencia) y una ficticia de COVID para abril–junio de 2020.

## Notas metodológicas y limitaciones

- **Cambio de encuesta en el empleo:** la desocupación y la informalidad vienen de la ENOE, salvo abril–junio de 2020 (encuesta telefónica ETOE) y julio 2020 – diciembre 2022 (ENOE nueva edición), según la nota de INEGI.
- **Estacionalidad:** la informalidad, las remesas, las ventas y varios precios (por ejemplo, trámites vehiculares a inicio de año) no están desestacionalizados. Las ficticias de mes absorben ese patrón.
- **Cobertura del crédito:** el saldo automotriz solo incluye a la banca comercial; no cuenta las financieras de marca ni otros no bancarios.
- **Remesas en dólares:** están en dólares corrientes, sin convertir a pesos ni deflactar.
- **Tendencias:** varias series tienen tendencia de largo plazo (crédito, remesas, seguro), lo que puede inflar el R² y las correlaciones.

## Fuentes y fechas de consulta

Las fechas vienen del encabezado de cada archivo descargado. Las que dicen "por confirmar" no estaban en el archivo: anótenlas al subirlo al repositorio.

| Serie | Institución y sistema | Fecha de consulta |
| --- | --- | --- |
| Ventas de vehículos ligeros (RAIAVL) | INEGI, Registro Administrativo de la Industria Automotriz de Vehículos Ligeros (publicación del 07/08/2026) | Por confirmar |
| Inflación anual | Banco de México, SIE | 20/11/2026 |
| Tasa de desocupación | INEGI, Banco de Información Económica (BIE) | 20/11/2026 |
| Tasa objetivo | Banco de México, SIE | 20/11/2026 |
| IGAE | INEGI, BIE | Por confirmar |
| Precios por genérico del INPC | INEGI, [Índices de precios, INPC Nacional (mensual)](https://www.inegi.org.mx/app/indicesdepreciosv2/Estructura.aspx?idEstructura=112001700030&T=%C3%8Dndices%20de%20Precios%20al%20Consumidor&ST=INPC%20Nacional%20(mensual)) | 24/11/2026 |
| Remesas | Banco de México, SIE, Balanza de pagos | 24/11/2026 |
| Tasa de informalidad (TIL1) | INEGI, [BIE, clave 444673](https://www.inegi.org.mx/app/indicadores/?tm=3&ind=444673#D444673_3838) | 24/11/2026 |
| Crédito automotriz | Banco de México, [SIE, cuadro CF835](https://www.banxico.org.mx/SieInternet/consultarDirectorioInternetAction.do?sector=19&accion=consultarCuadro&idCuadro=CF835&locale=es) | 24/11/2026 |
