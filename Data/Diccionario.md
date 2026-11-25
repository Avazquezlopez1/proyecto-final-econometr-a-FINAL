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

## Referencias

Formato APA 7.ª edición. Todas las series se consultaron el 20 de noviembre de 2026.

Banco de México. (2026a). *Balanza de pagos: Ingresos por remesas, remesas familiares totales (SE27803)* [Conjunto de datos]. Sistema de Información Económica. Recuperado el 20 de noviembre de 2026, de https://www.banxico.org.mx/SieInternet/

Banco de México. (2026b). *Banca comercial, cartera vigente otorgada al sector privado no bancario (metodología 2018) (CF835): Crédito automotriz (SF266105)* [Conjunto de datos]. Sistema de Información Económica. Recuperado el 20 de noviembre de 2026, de https://www.banxico.org.mx/SieInternet/consultarDirectorioInternetAction.do?sector=19&accion=consultarCuadro&idCuadro=CF835&locale=es

Banco de México. (2026c). *Índices de Precios al Consumidor y UDIS: Inflación, variación anual del INPC (SP30578)* [Conjunto de datos]. Sistema de Información Económica. Recuperado el 20 de noviembre de 2026, de https://www.banxico.org.mx/SieInternet/

Banco de México. (2026d). *Tasa objetivo (SF61745)* [Conjunto de datos]. Sistema de Información Económica. Recuperado el 20 de noviembre de 2026, de https://www.banxico.org.mx/SieInternet/

Instituto Nacional de Estadística y Geografía. (2026a). *Indicador Global de la Actividad Económica, series desestacionalizadas, base 2018* [Conjunto de datos]. Banco de Información Económica. Recuperado el 20 de noviembre de 2026, de https://www.inegi.org.mx/app/indicadores/?tm=3

Instituto Nacional de Estadística y Geografía. (2026b). *Índice Nacional de Precios al Consumidor, base segunda quincena de julio 2018: INPC Nacional (mensual), por objeto del gasto* [Conjunto de datos]. Recuperado el 20 de noviembre de 2026, de https://www.inegi.org.mx/app/indicesdepreciosv2/Estructura.aspx?idEstructura=112001700030

Instituto Nacional de Estadística y Geografía. (2026c). *Registro Administrativo de la Industria Automotriz de Vehículos Ligeros (RAIAVL): Venta al público de vehículos ligeros por marca, modelo, segmento y país de origen, enero 2005 – julio 2026* [Conjunto de datos]. Recuperado el 20 de noviembre de 2026, de https://www.inegi.org.mx/investigacion/datosprimarios/raiavl/

Instituto Nacional de Estadística y Geografía. (2026d). *Tasa de desocupación, nacional, serie desestacionalizada (ENOE)* [Conjunto de datos]. Banco de Información Económica. Recuperado el 20 de noviembre de 2026, de https://www.inegi.org.mx/app/indicadores/?tm=3

Instituto Nacional de Estadística y Geografía. (2026e). *Tasa de informalidad laboral 1 (TIL1), nacional, población total (ENOE), clave 444673* [Conjunto de datos]. Banco de Información Económica. Recuperado el 20 de noviembre de 2026, de https://www.inegi.org.mx/app/indicadores/?tm=3&ind=444673
