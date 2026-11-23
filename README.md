# AI_DA_Mario — Predecir la energy de una canción

Proyecto final de ML. Autores: Mario Daniel De La Luz Aldana e Ian Gonzales Seefoo.

## Dataset
Top 100 canciones más reproducidas en Spotify (`data/top 100 streamed_songs.csv`). Cada fila es una canción (o una versión/remix) con `id`, `name` y 12 variables de audio (`duration`, `energy`, `key`, `loudness`, `mode`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`, `danceability`). No hay valores faltantes; 14 filas repetidas por `id` se eliminan en el modelo final (quedan 86 canciones).

> Fuente original: [Top 100 Most Streamed Songs on Spotify (Kaggle, pavan9065)](https://www.kaggle.com/datasets/pavan9065/top-100-most-streamed-songs-on-spotify)

## Problema
- **y:** `energy` (0 a 1)
- **X:** las otras 11 variables numéricas de audio (sin `id` ni `name`)
- **Tipo:** regresión (y es un número continuo)
- **Métrica:** MAE, en unidades de energy (0 a 1). Split 60/20/20 (51 / 17 / 18 canciones), `random_state=42`.

## Resultados

| Modelo | MAE train | MAE validación | MAE test |
|---|---|---|---|
| Baseline (promedio) | 0.125 | 0.175 | — |
| **Regresión lineal ✓** | 0.051 | 0.095 | 0.105 |
| KNN (k = 7) | 0.075 | 0.123 | — |
| Árbol de decisión (max_depth = 3) | 0.043 | 0.120 | — |
| Random Forest (max_depth = 4) | 0.033 | 0.130 | — |

El modelo se eligió por validación; el test se evaluó una sola vez. Con 0.105 de MAE en test, el modelo se equivoca en promedio por 0.10 de energy (baseline: 0.130).

## Conclusión
- La regresión lineal mejora al baseline (~19% menos error en test), pero el error sigue siendo grande.
- `loudness` (+) y `acousticness` (−) son las variables que más pesan, igual que en el EDA.
- Árbol y Random Forest hacen overfitting con tan pocos datos.
- Con 86 canciones, validación y test son muy chicos (R² 0.73 en validación vs. 0.16 en test): los números son orientativos.
- Mejoras: más canciones, validación cruzada, más variables (género, año).

## Estructura
- `notebooks/explorar_canciones.ipynb` — EDA (Fase 1)
- `notebooks/modelo_final.ipynb` — split, modelos, test y feature importance
- `figures/` — gráficas usadas en la presentación
- `slides/` — presentación (PPTX y PDF)

## Cómo correrlo
```bash
pip install pandas matplotlib scikit-learn
jupyter notebook notebooks/modelo_final.ipynb   # Run All
```
