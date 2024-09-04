# Análisis financiero del sector automovilístico Mundial
Análisis financiero comparativo en el que se miden los ratios de empresas del sector automovilístico y se derivan conclusiones sobre la mejor inversión.

Consultar [informe_final](https://github.com/tommcrojo/analisis_financiero_coches/blob/main/informes/informe_final.md) para ver las conclusiones

Datos extraídos desde [https://discountingcashflows.com](https://discountingcashflows.com/company/TSLA/income-statement/)
## Limpieza de Datos
### Conversión de Divisas
Tuve que hacer un cambio de divisas, ya que los datos de Toyota estaban en JPY, y Vokswagen en EUR. Por lo tanto, convertí todos los datos a USD utilizando este código:
```python
# Suponiendo que estas son las tasas de conversión:
eur_to_usd = 1.11  # Ejemplo de tasa de EUR a USD
jpy_to_usd = 0.007  # Ejemplo de tasa de JPY a USD

# Para el DataFrame de volkswagen (eur)
for col in volkswagen_clean.columns[1:]:
    # Verificamos si la columna es numérica
    if pd.api.types.is_numeric_dtype(volkswagen_clean[col]):
        # Convertir los valores de EUR a USD
        volkswagen_clean[col] = volkswagen_clean[col] * eur_to_usd

# Para el DataFrame de toyota
for col in toyota_clean.columns[1:]:
    if pd.api.types.is_numeric_dtype(toyota_clean[col]):
        toyota_clean[col] = toyota_clean[col] * jpy_to_usd

# comprobamos que funciona
display(toyota_clean)
```
### Unión de tablas (Merging)
```python
cardata_a= tsla_clean.merge(toyota_clean, "outer")
cardata_a= cardata_a.merge(volkswagen_clean, "outer")
cardata_a= cardata_a.merge(ford_clean, "outer")
cardata_b= cardata_a.merge(gm_clean, "outer")
display(cardata_b)
```
### Exportación a CSV
Para poder tener y transformar los datos aún más, decidí exportar la base de datos a csv para que, si un compañero lo necesitase para hacer otro análisis, pudiera realizarlo más rápido al tener toda la transformación, unión, y conversión de divisas, hecho.
```python
cardata_b.to_csv("04-full_car_data_cleaned.csv",index=False)
```
## Cálculo de Ratios en Excel
### Power Query
Utilicé Power Query para importar los datos desde el dataframe de Pandas para comprobar que los números tuvieran el formato correcto (en USD)
![image](https://github.com/user-attachments/assets/4260fd9b-4653-4d9b-97ed-6e4a11305970)
También dividí los items "per share" entre 1,000,000 ya que, durante la transformación de los datos, multipliqué toda la fila por un millón para que fuera más fácil realizar visualizaciones más tarde.
![image](https://github.com/user-attachments/assets/f9acb3b9-0d45-4f83-9045-fc23edbff6f8)

