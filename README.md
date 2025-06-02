# 🚗 Análisis Exploratorio de Viajes en Uber - Lima 

## Contexto
Este proyecto analiza datos de viajes realizados en Uber en la ciudad de Lima. El objetivo es identificar las **zonas y franjas horarias más eficientes** desde el punto de vista del negocio, maximizando la rentabilidad tanto para los conductores como para la plataforma.

Incluye limpieza profunda de datos, imputación estadística avanzada, visualizaciones y exploración de hipótesis de negocio.

## Enfoque
- Identificar los **momentos y lugares más rentables**
- Evaluar el impacto del **tiempo de espera** y la **distancia** sobre la eficiencia
- Explorar cómo las **condiciones climáticas** afectan la rentabilidad
- Proponer **estrategias operativas** basadas en datos para Uber

## Dataset
- **Fuente**: [Kaggle - Uber Peru Dataset]([https://www.kaggle.com/datasets/marcusrb/uber-peru-dataset](https://www.kaggle.com/datasets/marcusrb/uber-peru-dataset))
- **Registros**: 23111
- **Variables clave**:
  - Temporales: 'start_at', 'arrived_at', 'end_at'
  - Espaciales: coordenadas de inicio/fin, grilla de ubicación
  - Operativas: duración del viaje, espera, distancias
  - Económicas: `price` (variable objetivo)
  - Climáticas: temperatura, viento, nubosidad, precipitación

## Procesamiento de Datos
- Eliminación de columnas derivadas o redundantes
- Detección y reemplazo de valores atípicos
- Cálculo de nuevas métricas (`trip_calc`, `wait_calc`)
- Imputación de `price` con el algoritmo **MICE** (paquete `miceforest`) tras análisis de significancia y multicolinealidad

## Hallazgos Principales
1. **Zona + horario**: la combinación espacial-temporal influye significativamente en la eficiencia del viaje
2. **Tiempo de espera**: penaliza fuertemente la rentabilidad por minuto
3. **Horarios nocturnos y zonas céntricas**: mayor precio por kilómetro recorrido
4. **Outliers climáticos**: los viajes en condiciones extremas presentan precios erráticos

## Modelos Utilizados


## Exploración Visual
🔗 Dashboard

---


