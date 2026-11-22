# Siniestros viales en Bogotá: ¿podemos anticipar los más graves?

Proyecto final **DataXperience** · Ingeniería de Sistemas · Universidad Ean · Noviembre de 2026

Análisis exploratorio, estadística descriptiva y modelado predictivo de los siniestros viales ocurridos en Bogotá entre el **1 de junio de 2025 y el 1 de junio de 2026**, con datos oficiales de la Secretaría Distrital de Movilidad (2026).

---

## Integrantes

| Integrante | Programa | Rol en el proyecto |
|---|---|---|
| Joana Natali Duarte Arias | Ingeniería de Sistemas | Etapa 1: selección y limpieza de datos · Repositorio, coordinación y edición del video |
| Cristhian Camilo Castañeda Dávila | Ingeniería de Sistemas | Etapa 1: selección y limpieza de datos · Etapa 2: estadística descriptiva y hallazgos |
| Santiago Otero Velandia | Ingeniería de Sistemas | Etapa 3: visualización, modelo predictivo y presentación |

---

## Video y presentación

- 🎥 **Video de sustentación:** [Ver video]((https://youtu.be/86zOq-pxhFI))
- 🎞️ **Presentación en línea:** [Ver presentación](https://joanadrte-pixel.github.io/Siniestros-viales-bogota/presentacion/)
- 📄 **Presentación en PDF:** [Descargar](presentacion/presentacion_siniestros.pdf)

---

## Pregunta de negocio

¿Dónde, cuándo y en qué condiciones ocurren los siniestros viales más graves en Bogotá, y es posible predecir si un siniestro tendrá personas fallecidas?

Responderla permite orientar controles, campañas e intervenciones de infraestructura hacia los lugares, horarios y usuarios con mayor riesgo.

---

## Datos

| Característica | Detalle |
|---|---|
| Fuente | Secretaría Distrital de Movilidad, servicio público SIMUR (Secretaría Distrital de Movilidad, 2026) |
| Datos originales | 147.923 siniestros (2020–2026), 23 columnas, más 3 tablas de víctimas y actores viales |
| Periodo analizado | 1 de junio de 2025 al 1 de junio de 2026 |
| Dataset limpio | 12.097 siniestros · 13 columnas · 0 valores nulos |
| Variable objetivo | `con_fallecidos` (1 = hubo al menos un fallecido; 0 = no) |
| Diccionario de variables | [`datos/entrega_para_santiago.md`](datos/entrega_para_santiago.md) |

---

## Etapas del proyecto

1. **Selección, exploración y limpieza** (Joana y Cristhian)
    - **Selección y justificación:** elegimos datos públicos, reales y de nuestra ciudad, que combinan variables numéricas, categóricas y de fecha, y permiten aplicar todo el ciclo de la ciencia de datos que usamos como ingenieros de sistemas.
    - **Exploración:** revisamos tipos de variables, valores nulos, duplicados y la distribución inicial de los siniestros por hora.
    - **Limpieza:** estandarizamos textos, convertimos fechas en milisegundos, marcamos los datos faltantes como "SIN DATO" (sin inventar valores) y contamos las víctimas por siniestro sin duplicar registros. Cada decisión quedó en una tabla problema → solución.
2. **Estadística descriptiva y hallazgos** (Cristhian)
    - **Tendencia central y dispersión:** media, mediana, moda, rango, varianza, desviación estándar y coeficiente de variación. La media (1,60 lesionados) supera a la mediana (1), lo que indica **asimetría positiva**.
    - **Comparación de variables:** siniestros por localidad, día de la semana, hora, clase de siniestro y tipo de usuario vial.
    - **Atípicos e hipótesis:** 461 atípicos (3,8 %) con la **regla del IQR**, conservados por ser casos reales; tres hipótesis sobre la noche, la clase de siniestro y la concentración por localidades.
3. **Visualización, modelo y storytelling** (Santiago)
    - **Visualización:** gráficos de barras, histogramas y boxplots con títulos que comunican el hallazgo.
    - **Modelo:** regresión logística sobre la **variable dicotómica** `con_fallecidos`, con clases balanceadas porque solo el 5,4 % de los siniestros es fatal (Pedregosa et al., 2011).
    - **Evaluación e interpretación:** matriz de confusión, **recall**, **ROC-AUC**, **validación cruzada estratificada de 5 particiones (k-fold)** y ***odds ratio*** para identificar los factores de mayor riesgo.
    - **Storytelling:** la presentación sigue la estructura contexto → hallazgos → recomendaciones (Knaflic, 2015).

---

## Resultados principales

| Métrica / Hallazgo | Resultado |
|---|---|
| Siniestros con fallecidos | **658** casos (**5,44 %**) |
| Concentración territorial | Las 5 localidades principales (Kennedy, Suba, Engativá, Puente Aranda, Fontibón) agrupan el **46 %** |
| Factor nocturno (19:00–04:59) | **7,71 %** con fallecidos de noche vs. **4,53 %** de día |
| Usuarios más vulnerables | Motociclistas (**39,9 %**) y peatones (**37,6 %**) suman el **77,5 %** de los fallecidos |
| Modelo (regresión logística balanceada) | **Recall:** 58,33 % (77 de 132 siniestros fatales detectados en prueba) · **ROC-AUC:** 0,671 |
| Estabilidad del modelo (validación cruzada, 5 particiones) | **Recall promedio:** 58,2 % ± 2,5 %: el modelo es estable y no depende de una división particular de los datos |
| Principales factores de riesgo (*odds ratio*) | Volcamiento (4,34×) · Atropello (2,29×) · Ciudad Bolívar (1,80×) |

**Recomendaciones:** reforzar los controles nocturnos, priorizar la protección de peatones y motociclistas, e intervenir las localidades con más siniestros.

---

## Interpretación del equipo y aplicación profesional

| Integrante | Lo aprendido | Aplicación profesional |
|---|---|---|
| **Joana Natali Duarte Arias** · Cobranza tardía (*late stage*, banca) | Un análisis solo es confiable si los datos están limpios y cada decisión queda documentada; en cobranza, un teléfono inválido o una cuenta duplicada significa gestionar al cliente equivocado. | En cobranza de etapa tardía, aplicaría esta misma metodología a la cartera en mora: limpiar los datos de los clientes, describirlos por saldo, días de mora y contactabilidad, y predecir qué cuentas tienen mayor probabilidad de pago, para priorizar la gestión y asignar mejor las cuentas a las agencias. |
| **Cristhian Camilo Castañeda Dávila** · Ingeniería de Sistemas | Describir antes de predecir: la estadística y los atípicos revelan dónde y cuándo se concentra el riesgo. | En gestión de servicios TI, limpiaría los registros de incidentes, describiría las horas y módulos con más fallos y entrenaría un modelo para anticipar tickets críticos antes de que escalen. |
| **Santiago Otero Velandia** · Analítica y datos | Con clases desbalanceadas el *accuracy* engaña; el *recall* y la validación cruzada muestran el desempeño real. | En operación de sistemas o logística, limpiaría los eventos históricos, identificaría patrones de riesgo y usaría regresión logística balanceada para priorizar alertas de alto impacto y actuar a tiempo. |

---

## Cómo reproducir el proyecto

1. **Descomprimir los datos originales:** extrae los 4 archivos CSV de `datos/datos_originales.zip` (`siniestros.csv`, `victimas_lesionadas.csv`, `victimas_fallecidas.csv`, `actores_viales.csv`) dentro de la carpeta `datos/original/`.
2. **Abrir el notebook en Google Colab:** [`notebook/proyecto_siniestros.ipynb`](notebook/proyecto_siniestros.ipynb).
3. **Ejecutar todo** (*Entorno de ejecución → Ejecutar todo*). El notebook contiene las 3 etapas en orden; la Etapa 3 lee `datos/siniestros_limpio.csv` y también puede ejecutarse de forma independiente.

---

## Estructura del repositorio

```text
├── README.md
├── datos/
│   ├── datos_originales.zip        # Tablas originales SIMUR (descomprimir en datos/original/)
│   ├── siniestros_limpio.csv       # Dataset limpio (12.097 filas, 13 columnas)
│   └── entrega_para_santiago.md    # Diccionario de variables y resumen de las Etapas 1 y 2
├── notebook/
│   └── proyecto_siniestros.ipynb   # Notebook con las Etapas 1, 2 y 3
├── presentacion/
│   ├── index.html                  # Presentación web
│   └── presentacion_siniestros.pdf # Presentación en PDF
└── video/
    └── enlace_video.md             # Enlace al video de sustentación
```

---

## Herramientas

Python, Google Colab, pandas (McKinney, 2010), Matplotlib (Hunter, 2007), seaborn (Waskom, 2021) y scikit-learn (Pedregosa et al., 2011). La narrativa de la presentación sigue los principios de *storytelling* con datos de Knaflic (2015).

---

## Referencias

Hunter, J. D. (2007). Matplotlib: A 2D graphics environment. *Computing in Science & Engineering, 9*(3), 90–95. https://doi.org/10.1109/MCSE.2007.55

Knaflic, C. N. (2015). *Storytelling with data: A data visualization guide for business professionals*. Wiley.

McKinney, W. (2010). Data structures for statistical computing in Python. En S. van der Walt y J. Millman (Eds.), *Proceedings of the 9th Python in Science Conference* (pp. 56–61). https://doi.org/10.25080/Majora-92bf1922-00a

Pedregosa, F., Varoquaux, G., Gramfort, A., Michel, V., Thirion, B., Grisel, O., Blondel, M., Prettenhofer, P., Weiss, R., Dubourg, V., Vanderplas, J., Passos, A., Cournapeau, D., Brucher, M., Perrot, M. y Duchesnay, É. (2011). Scikit-learn: Machine learning in Python. *Journal of Machine Learning Research, 12*, 2825–2830.

Secretaría Distrital de Movilidad. (2026). *Accidentalidad análisis* [Conjunto de datos]. Sistema Integrado de Información sobre Movilidad Urbana (SIMUR). https://sig.simur.gov.co/arcgis/rest/services/Accidentalidad/AccidentalidadAnalisis/FeatureServer

Silva, C. (s.f.). *DataXperience: Clase introductoria a la ciencia de datos* [Repositorio]. GitHub. https://github.com/ProfeCamilaSilva/DataXperience

Waskom, M. L. (2021). seaborn: Statistical data visualization. *Journal of Open Source Software, 6*(60), 3021. https://doi.org/10.21105/joss.03021
