# Modelos Probabilísticos

Repositorio de clase para el curso **Modelos Probabilísticos**. La estructura está organizada por las 8 sesiones del sílabo y separa dos tipos de material:

- `docs/`: apuntes teóricos en Markdown, con definiciones, fórmulas, criterios de uso y actividades.
- `notebooks/`: notebooks autocontenidos en Python para demostración, simulación y práctica.
- `data_sources/`: fuentes de datos del material original para usar durante clase. Está incluida en git.
- `ejercicios/`: problemas por sesión y casos integradores.
- `soluciones/`: soluciones guía para uso docente o autoevaluación.
- `legacy/material_original/`: material previo conservado sin tracking de git.

## Orden sugerido

| Sesión | Unidad | Tema central | Teoría | Notebook |
|---:|---|---|---|---|
| 01 | Conceptos básicos | Experimento aleatorio, espacio muestral, eventos e independencia | `docs/sesion_01_conceptos_basicos_probabilidad.md` | `notebooks/01_conceptos_basicos_probabilidad.ipynb` |
| 02 | Conceptos básicos | Probabilidad condicional y probabilidad total | `docs/sesion_02_probabilidad_condicional_total.md` | `notebooks/02_probabilidad_condicional_total.ipynb` |
| 03 | Variables aleatorias | Variable aleatoria discreta y continua | `docs/sesion_03_variables_aleatorias.md` | `notebooks/03_variables_aleatorias.ipynb` |
| 04 | Variables aleatorias | PMF, PDF y distribución de probabilidad | `docs/sesion_04_distribuciones_pmf_pdf.md` | `notebooks/04_distribuciones_pmf_pdf.ipynb` |
| 05 | Variables aleatorias | Momentos, posición, dispersión y expectativa condicional | `docs/sesion_05_momentos_una_variable.md` | `notebooks/05_momentos_una_variable.ipynb` |
| 06 | Variables aleatorias | Momentos conjuntos y desigualdad de Schwarz | `docs/sesion_06_momentos_dos_mas_variables.md` | `notebooks/06_momentos_dos_mas_variables.ipynb` |
| 07 | Modelos de probabilidad | Modelos discretos y propiedades | `docs/sesion_07_modelos_discretos.md` | `notebooks/07_modelos_discretos.ipynb` |
| 08 | Modelos de probabilidad | Modelos continuos y teorema del límite central | `docs/sesion_08_modelos_continuos_tlc.md` | `notebooks/08_modelos_continuos_tlc.ipynb` |

## Preparación del entorno

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

Los notebooks no dependen de archivos externos: generan datos sintéticos reproducibles con semillas fijas. Esto permite ejecutar la clase completa desde cero, sin descargar datasets ni recuperar material legacy.

Si existe `data_sources/`, varios notebooks activan bloques adicionales con datos reales del material original. El inventario y el mapeo por sesión están en `docs/fuentes_datos.md`.

El diccionario probabilístico de datasets está en `docs/diccionario_datos.md`.

## Cómo usar el repo en clase

1. Antes de clase: leer el Markdown de la sesión y revisar la sección de fórmulas clave.
2. Durante clase: ejecutar el notebook correspondiente y modificar los parámetros de simulación.
3. Después de clase: resolver `ejercicios/sesion_XX.md` y completar las celdas marcadas como práctica.
4. Para autoevaluación o pauta docente: revisar `soluciones/sesion_XX.md`.

## Criterio de diseño

El material nuevo reutiliza el contenido previo de clase 1, probabilidad condicional, momentos, momentos conjuntos, modelos discretos y demanda, pero lo deja en una secuencia única y consistente. Las presentaciones quedan fuera de esta estructura porque el repositorio está enfocado en teoría escrita y código ejecutable.

## Alineación con material previo

La segunda revisión dejó cubiertos los bloques principales del material anterior:

- Clase 1: dados, álgebra de eventos, independencia, Bernoulli/frecuencia relativa y DNI.
- Clase 2: condicional, probabilidad total, Bayes, EVPI, Naive Bayes didáctico y Titanic desde cero.
- Sesiones 3-4: variables aleatorias, PMF/PDF/CDF y distribución empírica con demanda.
- Sesión 5: momentos, percentiles, dispersión, cumulantes conceptuales y expectativa condicional.
- Sesión 6: covarianza, correlación, Cauchy-Schwarz, PCA, autocovarianza, ARIMA, ARIMAX y VAR.
- Sesión 7: modelos discretos, churn, conteos, sobredispersión, multiclase, log-loss, costos asimétricos y focal loss.
- Sesión 8: modelos continuos, ajuste sobre tráfico, teorema del límite central y pérdidas para demanda.

## Refuerzo teoría-código

Los notebooks incluyen celdas **Lectura matemática** antes de bloques clave. Cada una explicita:

- distribución o modelo asumido,
- parámetro que se estima,
- supuesto que puede fallar,
- diagnóstico que conviene mirar.
