# Siniestros viales en Bogotá: ¿podemos anticipar los más graves?

Proyecto final **DataXperience** · Ingeniería de Sistemas · Universidad EAN  
**Equipo:** Joana · Cris · Santiago

Análisis exploratorio, estadística descriptiva y modelado predictivo sobre siniestros viales en Bogotá entre el **1 de junio de 2025 y el 1 de junio de 2026**, utilizando datos oficiales de la Secretaría Distrital de Movilidad (servicio público SIMUR / ArcGIS).

---

## Resultados principales

| Métrica / Hallazgo | Resultado |
|---|---|
| Dataset limpio | **12.097** siniestros · **13** columnas · **0** valores nulos |
| Siniestros con fallecidos (`con_fallecidos = 1`) | **658** casos (**5,44 %**) |
| Concentración territorial | Las 5 localidades principales (Kennedy, Suba, Engativá, Puente Aranda, Fontibón) agrupan el **46 %** |
| Factor nocturno (19:00 – 04:59) | **7,71 %** con fallecidos de noche vs. **4,53 %** de día |
| Usuarios más vulnerables | Motociclistas (**39,9 %**) y peatones (**37,6 %**) suman el **77,5 %** de los fallecidos |
| Modelo (Regresión Logística balanceada) | **Recall:** `58,33 %` (`77/132` fatales detectados en prueba) · **ROC-AUC:** `0,671` |
| Principales factores de riesgo (*Odds Ratio*) | Volcamiento (`4,34x`) · Atropello (`2,29x`) · Ciudad Bolívar (`1,80x`) |

---

## Cómo reproducir el proyecto

1. **Descomprimir los datos originales antes de ejecutar:**  
   Extrae los 4 archivos CSV contenidos en `datos/datos_originales.zip` (`siniestros.csv`, `victimas_lesionadas.csv`, `victimas_fallecidas.csv`, `actores_viales.csv`) dentro de la carpeta `datos/original/`.
2. **Abrir y ejecutar el notebook único:** [`notebook/proyecto_siniestros.ipynb`](notebook/proyecto_siniestros.ipynb)  
   Contiene las **3 etapas** en orden:
   - **Etapa 1:** selección, exploración y limpieza de datos (`datos/siniestros_limpio.csv`).
   - **Etapa 2:** estadística descriptiva, análisis de atípicos (IQR) e hipótesis.
   - **Etapa 3:** visualización (barras, histograma, boxplot), entrenamiento y evaluación de Regresión Logística balanceada, e interpretación de *Odds Ratios*.  
   *(La Etapa 3 lee directamente `datos/siniestros_limpio.csv`, por lo que también puede ejecutarse de manera independiente.)*

---

## Estructura del repositorio

```text
├── datos/
│   ├── datos_originales.zip        # Tablas originales SIMUR (descomprimir en datos/original/)
│   ├── siniestros_limpio.csv       # Dataset limpio (12.097 filas, 13 columnas)
│   └── entrega_para_santiago.md    # Diccionario de variables y resumen Etapas 1 y 2
├── notebook/
│   └── proyecto_siniestros.ipynb   # Notebook unificado con las Etapas 1, 2 y 3
├── presentacion/
│   └── index.html                  # Presentación web interactiva
└── video/
    └── enlace_video.md             # Enlace al video de sustentación
```
