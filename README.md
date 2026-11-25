[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Avazquezlopez1/proyecto-final-econometr-a-FINAL/blob/main/notebook/analisis_ventas_autos.ipynb)

# Venta de vehículos ligeros en México y multicolinealidad (2016–2025)

Proyecto de Econometría — Universidad Panamericana.
**Equipo 6:** Euken, Álvaro Vázquez y Diego.

## Pregunta

¿Cómo se asocian las condiciones de financiamiento, ingreso, empleo y precios con la venta mensual de vehículos ligeros en México, y qué tanto dificulta la multicolinealidad medir esas asociaciones?

## Respuesta corta

Las ventas se asocian sobre todo con la actividad económica y con el costo de usar un auto. Un punto más del IGAE se asocia con ventas **4.4% mayores** (IC 95%: 3.5% a 5.4%), y un punto más en el precio real de la gasolina con ventas **1.0% menores** (IC 95%: −1.3% a −0.8%), manteniendo lo demás constante. Ambos resultados se mantienen en todas las pruebas de robustez y con Ridge y Lasso.

La multicolinealidad sí dificulta responder la pregunta: al pasar de 4 a 13 variables, la tasa de interés, la inflación y la desocupación pierden significancia y cambian de signo, aunque el R² sube a 0.95. Quitar variables redundantes y usar Ridge la reducen, pero no la eliminan. Son asociaciones, no efectos causales.

## Datos

Base mensual de **120 observaciones** (enero 2016 – diciembre 2025), construida con series oficiales del INEGI y del Banco de México. La unidad de análisis es el mes, a nivel nacional.

| Tema | Variables | Fuente |
| --- | --- | --- |
| Variable dependiente | Venta al público de vehículos ligeros | INEGI, RAIAVL |
| Actividad y empleo | IGAE, desocupación, informalidad laboral | INEGI, BIE |
| Financiamiento | Tasa objetivo, crédito automotriz real | Banco de México, SIE |
| Ingreso de los hogares | Remesas | Banco de México, SIE |
| Precios | Inflación anual; precios reales de automóviles, gasolina, seguro, trámites, reparación y transporte público | Banco de México e INEGI (INPC) |

## Metodología

1. **Exploración:** estadísticas descriptivas, serie de tiempo, estacionalidad, gráficas de dispersión y matriz de correlaciones.

3. **Regresión por MCO** de log(ventas) con 11 variables ficticias de mes (enero como referencia) y una variable ficticia de COVID (abril–junio 2020). Se comparan cuatro especificaciones: base, ingreso y crédito, completa y reducida (el modelo principal).

5. **Errores estándar Newey-West** (4 rezagos), porque los residuos están autocorrelacionados.

7. **Diagnóstico:** VIF, experimento de eliminar una variable a la vez, y pruebas de Breusch-Pagan, Durbin-Watson, Ljung-Box y Jarque-Bera.

9. **Robustez:** tendencia lineal, desocupación en lugar del IGAE, muestra sin 2020 y errores estándar convencionales.

11. **Regularización:** Ridge y Lasso con validación cruzada temporal, como complemento frente a la multicolinealidad.

## Principales resultados

| Modelo | Variables explicativas | R² ajustado |
| --- | --- | --- |
| M1 Base | 4 | 0.678 |
| M2 Ingreso y crédito | 7 | 0.863 |
| M3 Completo | 13 | 0.933 |
| M4 Reducido (principal) | 9 | 0.922 |

- **Multicolinealidad:** en el modelo completo, las remesas (VIF 26), el IGAE (24) y el crédito (21) superan el umbral de 10. El modelo reducido baja la mayoría de los VIF, pero las remesas y el IGAE siguen altos por su tendencia común.
- **Robustez:** al agregar una tendencia lineal, el efecto negativo de las remesas desaparece y el crédito automotriz se vuelve positivo y significativo. Parte de esas asociaciones era crecimiento común en el tiempo.
- **Ridge y Lasso:** Ridge regresa la inflación y la desocupación a su signo esperado y reduce a la mitad el coeficiente de las remesas. Lasso solo elimina el precio de reparación.

## Cómo reproducir en google colab

da clic en el botón "Abrir en Colab" de arriba y luego en Entorno de ejecución, Ejecutar todas. Los datos se leen automáticamente desde este repositorio.

## Lista de verificación de la guía

- [x] La pregunta y la unidad de análisis son explícitas.
- [x] Las fuentes, variables, unidades y limpieza están documentadas.
- [x] La ecuación corresponde a los resultados presentados.
- [x] Los coeficientes se interpretan en unidades comprensibles (%).
- [x] Se discuten incertidumbre, supuestos y límites de la inferencia.
- [x] Se evalúan y contextualizan correlaciones y VIF.
- [x] Código, datos y tablas permiten reproducir el análisis.

## Limitaciones

Los resultados son asociaciones, no efectos causales. Las principales limitaciones son la posible causalidad inversa entre la tasa de interés y las ventas, variables omitidas como la escasez de semiconductores de 2021–2022, la tendencia común de varias series y el cambio de encuesta de empleo del INEGI en 2020.
