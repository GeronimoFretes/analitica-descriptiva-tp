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
- Cálculo de nuevas métricas (trip_calc, wait_calc)
- Imputación de price con el algoritmo MICE (paquete miceforest) tras análisis de significancia y multicolinealidad
- Enriquecimiento del dataset con información externa:
      - Datos meteorológicos (viento, temperatura, precipitación) extraídos por coordenadas usando variables como u10, v10, tp y tcc
      - Zonificación mediante la API de Google Maps, que permitió asignar una zona (distrito) a cada punto de inicio y fin de viaje a partir de latitud y longitud

## Hallazgos Principales
Se rechazaron tres de las cuatro hipótesis nulas planteadas. Para la Hipótesis 1, se confirmó que la combinación de zona y franja horaria influye significativamente en la rentabilidad del viaje (p < 0.001). La Hipótesis 2 también fue validada: el precio por kilómetro es mayor en zonas de alta demanda (p < 0.001). La Hipótesis 4 fue confirmada, mostrando diferencias de rentabilidad según la hora del día (p < 0.001). En cambio, no se rechazó la Hipótesis 3, ya que no se encontró una relación significativa entre el tiempo de espera y el precio del viaje (p = 0.667).

## Modelos Utilizados
Aplicamos un *modelo de gravedad* para estimar la cantidad de viajes entre zonas de Lima, en función de su nivel de actividad y la distancia entre ellas. Este enfoque permite predecir la demanda de movilidad y puede ser útil para optimizar la asignación de conductores en plataformas como Uber.

## Exploración Visual
Dashboard adjuntado al repositorio como Uber_Peru_2010.pbix

---


