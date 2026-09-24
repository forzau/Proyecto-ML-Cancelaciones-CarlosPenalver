# Machine Learning 

## Proyecto: predicción de cancelaciones hoteleras

El cuaderno principal estima el riesgo de cancelación de una reserva para ayudar a priorizar su revisión por parte de Revenue Management. Parte de un CSV con 119.390 reservas y 32 columnas y compara una regresión logística con árboles de decisión, Random Forest, HistGradientBoosting y XGBoost.

El trabajo incluye revisión de calidad de datos, análisis exploratorio, ingeniería de variables, preprocesamiento, validación temporal, selección del umbral de decisión y explicaciones con SHAP. La presentación resume el caso.

### Resultados

El modelo seleccionado es **XGBoost**, con un umbral de alerta de **0,25**. En las **23.831 reservas elegibles** del periodo de test (enero a agosto de 2017) obtuvo:

| Métrica | Valor |
| --- | ---: |
| AUC-ROC | 0,764 |
| F1 | 0,586 |
| Precisión | 0,450 |
| Recall | 0,840 |

Detectó **6.224 de 7.409 cancelaciones** y señaló **7.611 reservas que finalmente no se cancelaron**. En total, generó alertas para el **58,1 %** de las reservas evaluadas. Son resultados predictivos; el cuaderno no estima ahorro económico ni demuestra que una intervención evite cancelaciones.

### Cómo ejecutar el cuaderno principal

Se necesita Python y Jupyter. Desde la raíz del repositorio, crea un entorno e instala las bibliotecas usadas en el análisis:

```bash
python -m venv .venv
```

Activa el entorno en PowerShell con `.\.venv\Scripts\Activate.ps1` o, en macOS/Linux, con `source .venv/bin/activate`. Después:

```bash
python -m pip install jupyterlab pandas numpy matplotlib scipy scikit-learn xgboost shap
cd Proyecto
jupyter lab proyecto_cancelaciones_hoteleras.ipynb
```

El cuaderno espera que `reservas_hoteleras - reservas_hoteleras.csv` esté en su mismo directorio. Ejecuta las celdas en orden.

## Alcance y limitaciones

- La fecha de reserva se estima a partir de la fecha de llegada y `lead_time`; el archivo no contiene un historial completo de cambios de cada reserva.
- La disponibilidad **al reservar** de algunos predictores no está verificada. Hay que comprobarla antes de usar el modelo en un proceso real.
- Las métricas del test corresponden solo a las reservas elegibles: **2.734 de 26.565** reservas del periodo quedaron fuera por las reglas de calidad y de ADR definidas en el análisis.
- Los datos proceden de dos hoteles y de un periodo histórico concreto. Antes de aplicar el modelo a otros hoteles o fechas, hace falta una validación nueva.

El cuaderno principal documenta las decisiones metodológicas, los resultados completos y una propuesta de piloto para evaluar posibles acciones sobre las reservas de mayor riesgo.
