# Análisis de Viajes en Uber Lima-Callao (2010)

Flujo de trabajo para explorar, modelar y extraer inteligencia de negocio
a partir de la base de viajes Uber 2010 en Lima y Callao.  
Se cubren desde las limpiezas iniciales hasta un modelo gravitacional y varios análisis
descriptivos, inferenciales y de visualización.

## [Link a la presentación en Canva](https://www.canva.com/design/DAGpDdtC3YI/AJRk-RocXl32Q2szMvzTvw/edit)

## Dashboard
En el archivo Uber_Peru_2010.pbix

## Dataset
- **Fuente**: [Kaggle - Uber Peru Dataset](https://www.kaggle.com/datasets/marcusrb/uber-peru-dataset)
- **Registros**: 23111
- **Variables clave**:
  - Temporales: 'start_at', 'arrived_at', 'end_at'
  - Espaciales: coordenadas de inicio/fin, grilla de ubicación
  - Operativas: duración del viaje, espera, distancias
  - Económicas: 'price'

## Procesamiento de Datos
- Enriquecimiento del dataset con información externa:
      - Datos meteorológicos (viento, temperatura, precipitación) extraídos por coordenadas usando variables como u10, v10, tp y tcc
      - Zonificación mediante la API de Google Maps, que permitió asignar una zona (distrito) a cada punto de inicio y fin de viaje a partir de latitud y longitud
- Detección y reemplazo de valores atípicos
- Cálculo de nuevas métricas (tiempo de espera, duración del viaje)
- Imputación de price con el algoritmo MICE (paquete miceforest) tras análisis de significancia y multicolinealidad

## Modelo de Gravedad Implementado
Construimos un modelo de gravedad estimado por Poisson Pseudo-Maximum Likelihood (PPML) para explicar los viajes entre distritos usando: distancia centro-a-centro, población, kilómetros de calle y número de nodos viales (intersecciones) en origen y destino. El ajuste base (pseudo-R² ≈ 0,21; RMSE ≈ 19) ya permite estimar cómo variará la demanda si se acorta la distancia media o se incrementa la infraestructura (calles o nodos).

Añadiendo efectos fijos de origen y destino obtenemos un modelo de alta fidelidad (pseudo-R² ≈ 0,97; RMSE ≈ 18,6) que devuelve dos coeficientes operativos:

* γᵢ → **potencial de salida** distritos con alto γ necesitan más conductores/puntos de enganche para capturar demanda latente.
* δⱼ → **potencial de llegada** zonas con alto δ son candidatas a promociones geolocalizadas ( descuento al destino ) o a incentivos de “viaje compartido” para absorber el flujo entrante.

En conjunto, el modelo permite simular escenarios de infraestructura, re-asignar oferta de conductores y diseñar campañas localizadas que maximicen el monto total cobrado por viajes y reducen tiempos de espera para los usuarios.

## Hallazgos principales

1. **Optimizar incentivos por franja horaria**
   La franja pico de **06-09 h** genera la mayor rentabilidad.

2. **Ajustar pricing según nivel de demanda zonal**
   Las 10 zonas más activas cobran, en promedio, un precio por km significativamente mayor que las de baja demanda. → Mantener la tarifa base elevada en estas áreas y lanzar campañas de captación en las zonas de menor demanda.

3. **Reposicionar flota con micro-ventanas rentables**
   \*Combinaciones concretas zona × franja concentran rentabilidad:

   * **Callao • madrugada**, **Miraflores • mañana**, **La Molina • atardecer** → notificar a conductores cercanos y mostrar “hotspots” en el mapa.
   * **Magdalena** y **Jesús María** en tarde/noche registran márgenes bajos → priorizar promociones para estimular demanda.\*

4. **Usar el modelo de gravedad para expansión y simulación**
   \*El modelo explica **97 %** de los flujos viaje-a-viaje:

   * Elasticidad distancia = -0.29 → viajes caen \~29 % por cada 10 % extra en distancia.
   * Mayor población y km de calles elevan la demanda.
     → Herramienta para prever impacto de nuevas vías, cierres o tarifas por atracción/distrito y para decidir dónde abrir nuevas zonas de servicio o re-asignar flota.\*

5. **Índice de exportación de viajes**
   Distritos dormitorio (C. de la Legua, La Perla, Independencia…) exportan > 90 % de sus trayectos en hora punta → planificar zonas de espera inteligentes para reducir tiempos muertos de los socios.

6. **Capacidad vial infra-optimizada**
   Zonas con poca densidad de nodos viales pero alto flujo (Surquillo, San Isidro) son candidatas a micro-reordenamiento de paradas y carriles exclusivos para pick-ups.

## Próximos pasos

1. **Aumentar los datos a múltiples años** para capturar dinámicas temporales y recalibrar el modelo gravitacional anualmente.
2. Incorporar **datos de tráfico en tiempo real** (Waze, Google) para refinar costos generalizados.