# Entrega Etapas 1 y 2 → Etapa 3 (Santiago)

Proyecto: **Siniestros viales en Bogotá: ¿podemos anticipar los más graves?**
Archivo de trabajo: `datos/siniestros_limpio.csv` · Notebook: `notebook/proyecto_siniestros.ipynb`

---

## 1. Dataset limpio

| Dato | Valor |
|---|---|
| Fuente | Secretaría Distrital de Movilidad de Bogotá, servicio público SIMUR (ArcGIS) |
| Periodo | 1 de junio de 2025 al 1 de junio de 2026 |
| Filas | 12.097 siniestros (uno por fila, sin duplicados) |
| Columnas | 13 |
| Nulos | 0 |
| Codificación | UTF-8, separador coma |

## 2. Diccionario de columnas

| Columna | Tipo | Descripción | Valores / rango |
|---|---|---|---|
| `codigo_accidente` | entero | Identificador único del siniestro | Único por fila |
| `fecha` | texto (DD/MM/AAAA) | Fecha del siniestro | 01/06/2025 – 01/06/2026 |
| `hora` | texto (HH:MM) | Hora del siniestro | 00:00 – 23:59 |
| `dia_semana` | categórica | Día de la semana | Lunes a Domingo |
| `gravedad` | categórica | Gravedad oficial | CON HERIDOS (10.641), SOLO DANOS (798), CON MUERTOS (658) |
| `clase_accidente` | categórica | Tipo de siniestro | CHOQUE, ATROPELLO, CAIDA DE OCUPANTE, VOLCAMIENTO, OTRO |
| `localidad` | categórica | Localidad de Bogotá | 20 localidades (1 registro como SIN DATO) |
| `barrio` | categórica | Barrio | 902 barrios |
| `latitud` | decimal | Coordenada | 4,41 – 4,82 |
| `longitud` | decimal | Coordenada | −74,21 – −74,01 |
| `total_lesionados` | entero | Personas lesionadas en el siniestro | 0 – 41 |
| `total_fallecidos` | entero | Personas fallecidas en el siniestro | 0 – 4 |
| `con_fallecidos` | binaria (0/1) | **Variable objetivo del modelo**: 1 = hubo al menos un fallecido | 1 = 658 (5,44 %), 0 = 11.439 |

> ⚠️ Para el modelo, **no usar** `gravedad` ni `total_fallecidos` como variables predictoras: contienen la respuesta (fuga de información) y el modelo saldría "perfecto" sin aprender nada.

## 3. Limpieza realizada (problema → solución)

| Problema | Solución | Registros afectados |
|---|---|---|
| Textos con espacios, mayúsculas o tildes inconsistentes | Estandarizados (sin espacios sobrantes, mayúsculas, sin tildes) | 147.923 |
| Fecha en milisegundos Unix y hora con segundos | Convertidas a fecha DD/MM/AAAA y hora HH:MM | 147.923 |
| Registros fuera del periodo de análisis | Eliminados para trabajar un año (2020–2026 → jun 2025–jun 2026) | 135.826 |
| Localidad vacía | Marcada como SIN DATO (no se inventa el valor) | 1 |
| Lesionados y fallecidos estaban en otras tablas (una fila por persona) | Contados por siniestro y agregados con `map`, sin duplicar siniestros | 12.097 |
| Columnas innecesarias (IDs internos, fechas en milisegundos, coordenadas repetidas) | Eliminadas; quedan 13 columnas | 10 columnas |

Comprobaciones de coherencia: 0 siniestros CON HERIDOS sin lesionados, 0 SOLO DANOS con lesionados y 0 CON MUERTOS sin fallecidos.

## 4. Resultados de la Etapa 2

**Tendencia central y dispersión**

| Medida | Lesionados por siniestro | Hora (0–23) |
|---|---|---|
| Media | 1,60 | 12,57 |
| Mediana | 1 | 13 |
| Moda | 1 | 7 |
| Rango | 41 | 23 |
| Desviación estándar | 1,21 | 6,02 |
| Coeficiente de variación | 76 % | 48 % |
| IQR | 1 | 10 |
| Asimetría | 6,29 | −0,13 |

- Media > mediana: asimetría positiva; pocos siniestros con muchas víctimas elevan el promedio, por eso se reporta la mediana.
- 461 atípicos por IQR (3,81 %); se conservan porque son siniestros reales y graves. El máximo (41 lesionados, 15/08/2025, Kennedy) queda señalado para revisión.

**Comparaciones clave**

- Las 5 localidades con más siniestros (Kennedy, Suba, Engativá, Puente Aranda, Fontibón) concentran el 46 %.
- Día con más siniestros: viernes. Horas pico: 6:00, 7:00 y 17:00.
- Fallecidos por tipo de usuario: motociclistas 39,9 % y peatones 37,6 %, aunque representan 31,9 % y 10,3 % de las personas involucradas.

**Hipótesis para la Etapa 3**

| Hipótesis | Evidencia |
|---|---|
| H1 · De noche hay más siniestros fatales | 7,71 % con fallecidos de noche vs 4,53 % de día; fin de semana casi igual que entre semana (5,78 % vs 5,31 %) |
| H2 · La clase de siniestro predice la gravedad | Volcamiento 18,1 % y atropello 10,1 % con fallecidos vs choque 4,0 % |
| H3 · Pocas localidades concentran los siniestros | Top 5 = 46 % del total |

## 5. Sugerencias para el modelo (Etapa 3)

- **Objetivo:** `con_fallecidos` → regresión logística.
- **Predictoras sugeridas:** hora (o franja día/noche), día de la semana, clase de siniestro, localidad.
- **Clases desbalanceadas (5,4 % positivos):** usar `class_weight='balanced'`, `stratify=y` y evaluar con **recall, F1 y AUC**, no solo accuracy.
