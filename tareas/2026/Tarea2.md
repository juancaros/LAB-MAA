<font size="3">**Tarea 2**</font>

<u> *Instrucciones* </u>

Los resultados de los ejericicios propuestos se deben entregar como un notebook por correo electronico a *juancaros@udec.cl* el dia 25/5 hasta las 21:00. Es importante considerar que el código debe poder ejecutarse en cualquier computadora con la data original del repositorio. Recordar la convencion para el nombre de archivo ademas de incluir en su documento titulos y encabezados por seccion. 

El archivo a utilizar es *dataset_prueba.csv*, que contiene un conjunto de variables a nivel de ciudad durante el periodo de COVID-19, incluyendo caracteristicas sociodemograficas, movilidad, restricciones gubernamentales, casos de COVID-19, movilidad y busquedas en Google para terminos espeficicos.
**Variable dictionary**

    - "iso_code": "Country or region code",
    - "date": "Date of the record",
    - "retail_and_recreation_percent_change_from_baseline": "Change in retail/recreation activity",
    - "grocery_and_pharmacy_percent_change_from_baseline": "Change in grocery/pharmacy activity",
    - "parks_percent_change_from_baseline": "Change in park visits",
    - "transit_stations_percent_change_from_baseline": "Change in transit station activity",
    - "workplaces_raw": "Raw workplace activity data",
    - "residential_percent_change_from_baseline": "Change in residential activity",
    - "trend": "Trend indicator",
    - "workplaces": "Processed workplace activity data",
    - "Valor_Stringency_Index": "Stringency index value",
    - "Valor_GovernmentResponseIndex": "Government response index value",
    - "Valor_EconomicSupportIndex": "Economic support index value",
    - "Valor_Containment_Health_index": "Containment and health index value",
    - "workplace_closing": "Workplace closing indicator",
    - "daily_cases": "Daily reported cases",
    - "week": "Week number",
    - "year": "Year",
    - "CODE": "Region code",
    - "NAME": "Region name",
    - "Population": "Population of the region",
    - "agriculture": "Agriculture sector data",
    - "industry": "Industry sector data",
    - "construction": "Construction sector data",
    - "age_dependency": "Age dependency ratio",
    - "old_age_dependency": "Old age dependency ratio",
    - "young_age_dependency": "Young age dependency ratio",
    - "sex_ratio": "Sex ratio",
    - "unemp": "Unemployment rate",
    - "f_unemp": "Female unemployment rate",
    - "m_unemp": "Male unemployment rate",
    - "foreigners": "Foreign population percentage",
    - "country": "Country name"

Preguntas:

1. Cargar la base de datos en el ambiente. Identifique los tipos de datos que se encuentran en la base, realice estadisticas descriptivas sobre las variables importantes (Hint: Revisar la distribuciones, datos faltantes, outliers, etc.) y limpie las variables cuando sea necesario. 

2. Ejecute un modelo Pooled OLS para estimar la relacion entre las restricciones gubernamentales de movilidad y la variacion en movilidad laboral. Seleccione las variables independientes a incluir en el modelo final e interprete su significado.

3. Ejecute un modelo efectos fijos para estimar la misma relacion anterior. Seleccione las variables independientes a incluir en el modelo final e interprete su significado.

4. Ejecute un modelo de efectos aleatorios para estimar la misma relacion anterior. Seleccione las variables independientes a incluir en el modelo final e interprete su significado. 

5. Comente los resultados obtenidos en 2, 3 y 4. ¿Cuáles y por qué existen las diferencias entre los resultados?. En su opinión, ¿Cuál sería el más adecuado para responder la pregunta de investgación y por qué? ¿Qué variables resultaron ser robustas a la especificación?

6. Ejecute un modelo de efectos aleatorios correlacionados (CRE) para estimar la misma relacion anterior. Seleccione las variables independientes a incluir en el modelo final e interprete su significado. Es este modelo adecuado, dada la data disponible, para modelar el componente no observado?

7. Usando sus respuestas anteriores, que modelo prefiere? que se puede inferir en general respecto del efecto de las restricciones gubernamentales sobre la movilidad laboral?

8. Control Sintetico: Es posible que sus resultados anteriores tengan sesgo dado que la movilidad laboral y las restricciones son parte de un fenomeno dinamico en el tiempo. Utilice el notebook SynthControl.ipynb para estimar el efecto causal de las restricciones gubernamentales y la movilidad para una ciudad de su eleccion, usando control sintetico (viendo las fechas semanales, deben elegir el periodo de tratamiento e indentificar los controles potenciales para la ciudad elegida, que no puede ser Zaragoza). Defina las variables para calcular el control sintetico y discuta sus resultados (instalar libreria pysyncon)