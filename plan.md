# Trabajo Machine Learning — Vacunación COVID-19 y Mortalidad por Zona (Chile)

## Contexto
Pregunta de investigación

¿Cómo influyó el porcentaje de vacunación contra COVID-19 en la mortalidad en las
distintas zonas del país (Norte, Centro, Sur)?

Modelo propuesto

Modelo de regresión lineal panel (zona × tiempo):

```text
y_zona,t = a + b_zona · X_zona,t + e_zona,t
```

Donde:

- y_zona,t: mortalidad en la zona "zona en el período" t, medida como
  tasa de fallecidos por 100.000 habitantes (no el total absoluto, porque
  las zonas tienen tamaños poblacionales muy distintos — el Centro concentra
  la Región Metropolitana — y comparar totales sesgaría el resultado a favor
  de la zona más poblada).
- X_zona,t: porcentaje de población vacunada (al menos 1ª dosis, o esquema
  completo, a definir) en la zona zona hasta el período t.
- b_zona: coeficiente específico por zona (o interacción Zona × X), para
   poder comparar si el efecto de la vacunación difiere entre Norte, Centro y Sur.

Supuestos

1. Unidad temporal común: se agregan ambas series a frecuencia semanal
2. La tasa de vacunación es exógena respecto a la mortalidad contemporánea
   dentro de cada zona (no hay retroalimentación inmediata mortalidad→vacunación
   en la misma semana).
3. Independencia entre zonas
4. Los fallecidos y la vacunación están correctamente asignados a la comuna/
   centro de residencia o atención**, y esa comuna se mapea de forma unívoca a
   una región y a una de las 3 zonas.
5. Zonificación usada:
   - Norte: Arica y Parinacota, Tarapacá, Antofagasta, Atacama, Coquimbo
   - Centro: Valparaíso, Metropolitana, O'Higgins, Maule
   - Sur: Ñuble, Biobío, La Araucanía, Los Ríos, Los Lagos, Aysén, Magallanes

## Extracción y Transformación (ETL)
fuente: data/COVID/datos-covid-19. El notebook utiliza exclusivamente los
tres archivos siguientes y carga solo las columnas necesarias:

- data/COVID/datos-covid-19/producto84/fallecidos_comuna_edad_totales_std.csv | Region, Fecha, Total |
- data/COVID/datos-covid-19/producto83/vacunacion_establecimiento_std.csv | Establecimiento, Fecha, Dosis, Cantidad |
- data/COVID/datos-covid-19/producto93/ContactosPorComuna.csv | Region, Codigo comuna, Comuna, Poblacion |

No se deben recorrer ni cargar otros productos o archivos de data/COVID.

Pasos

1. Fallecidos: agregar primero `Total` por `Region + Fecha`, colapsando
   `Comuna` y `Edad` antes de cualquier transformación costosa. Mapear la
   región a una zona y agregar a semanas con inicio el lunes.
2. Población: convertir `Poblacion` a numérico, conservar un único valor por
   `Codigo comuna`, mapear la región a zona y sumar por zona. También guardar
   el resumen por región y zona.
3. Vacunación: usar `vacunacion_establecimiento_std.csv`. Si `Dosis` contiene
   varias categorías, conservar los registros de primera dosis; si contiene
   una sola categoría (como `Personas_vacunadas`), conservar todos los
   registros porque el archivo ya representa la medida disponible. Agregar
   primero por `Establecimiento + Fecha`.
4. Asignación geográfica de vacunación: como el archivo no trae región,
   normalizar el nombre del establecimiento y buscar el nombre de la comuna
   dentro de los establecimientos. Usar una expresión regular compilada y
   mapear cada establecimiento único a Norte, Centro o Sur. Guardar los
   establecimientos no asignados como auditoría.
5. Tasas: `tasa_mortalidad = fallecidos / poblacion * 100000` y
   `pct_vacunacion = vacunados_acumulados / poblacion * 100`.
6. Unión final: hacer `merge` de mortalidad y vacunación por `Zona + semana`,
   agregar la población de la zona y generar el panel zona × semana listo
   para la regresión.

Optimización y salidas

- La lectura usa `usecols` y no se realiza un escaneo recursivo de la carpeta.
- Las tablas grandes se agregan antes de mapear zonas o fechas cuando es
  posible.
- El notebook genera en `outputs/COVID`:
  `data_catalog.csv`, `data_quality_report.csv`,
  `covid_zone_weekly_panel.csv`, `population_by_region_zone.csv`,
  `vaccination_unmapped_establishments.csv`, `zone_vaccination_ols.csv` y
   `validation_report.json`.
