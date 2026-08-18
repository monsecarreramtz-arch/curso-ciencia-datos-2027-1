# Laboratorio 02 — Análisis de pacientes Synthea

## Descripción

Análisis de un dataset sintético de historias clínicas de Synthea. Se realizaron tareas de carga, optimización de memoria, auditoría de calidad, uniones, análisis clínicos y comparación entre pandas, Polars y PySpark.

## Datos utilizados

Los datos se descargaron desde:

- Página: https://synthea.mitre.org/downloads
- Dataset: **1K Sample Synthetic Patient Records, CSV**
- Descarga: https://mitre.box.com/shared/static/aw9po06ypfb9hrau4jamtvtz0e5ziucz.zip
- Mirror: https://synthetichealth.github.io/synthea-sample-data/downloads/synthea_sample_data_csv_apr2020.zip

Archivos analizados:

- `patients.csv`: 1,163 pacientes
- `encounters.csv`: 61,459 encuentros
- `observations.csv`: 531,144 observaciones

Los CSV no se incluyen en el repositorio y deben descargarse desde los enlaces anteriores.

## Estructura esperada

```text
lab02/
├── analisis_pacientes.ipynb
├── README.md
└── data/
    └── raw/
        ├── patients.csv
        ├── encounters.csv
        └── observations.csv
```

Si los archivos se encuentran en otra ubicación, debe modificarse la variable `RAW` al inicio del notebook.

## Requisitos

- Python 3.10 o posterior
- Java 17 o posterior
- Jupyter Notebook

Instalación de bibliotecas:

```bash
pip install pandas numpy pyarrow matplotlib polars pyspark psutil
```

## Reproducción

1. Descargar y descomprimir el dataset.
2. Colocar los tres CSV en `data/raw/`.
3. Instalar las bibliotecas requeridas.
4. Iniciar Jupyter con `python -m notebook`.
5. Abrir `analisis_pacientes.ipynb`.
6. Seleccionar **Kernel → Restart Kernel and Run All Cells**.
7. Verificar que todas las celdas terminen sin errores.

## Resultados principales

- `observations.csv` pasó de 337.36 MB a 19.59 MB.
- La reducción de memoria fue de 94.19%.
- No se encontraron identificadores de pacientes duplicados.
- Las uniones conservaron las 531,144 observaciones.
- El procesamiento por lotes tuvo un pico medido de 5.90 MB.
- Los tres motores produjeron los mismos resultados clínicos.
- Se definió un desempate secundario por `CODE` para los códigos con igual frecuencia.

## Comparación de herramientas

| Herramienta | Líneas de código | Tiempo (s) | Memoria pico incremental (MB) | Qué costó más |
|---|---:|---:|---:|---|
| pandas | 145 | 1.45 | 113.80 | Controlar dtypes y transformaciones intermedias |
| Polars | 133 | 0.69 | 156.79 | Adaptarse a la sintaxis basada en expresiones |
| PySpark | 166 | 22.48 | 376.17 | Configurar Java, evaluación diferida y caché |

La memoria corresponde al incremento máximo observado sobre la memoria inicial de cada ejecución e incluye Python y sus procesos hijos.

## Recomendación

Para este volumen se recomienda pandas por su facilidad de uso y rendimiento suficiente. Polars es conveniente cuando el volumen o el tiempo de ejecución aumentan. PySpark se reservaría para datos que no quepan en una computadora o necesiten procesamiento distribuido.
