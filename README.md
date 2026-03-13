# Telecom-X-Parte-2

Repositorio del Desafío 'Telecom X: Análisis de evasión de clientes - Parte 2' del Curso de Programación de ONE (Oracle Next Education), formación en Data Science, dictado por Oracle y Alura LATAM.

**Autor:** Gabriel Sorrentino

## Introducción

Tras los resultados del análisis hecho en el challenge anterior, la empresa Telecom X LATAM me incluyó en el equipo de Machine Learning, en el que debí procesar y hacer un conjunto de análisis con los datos del challenge anterior, eliminar filas y pasarlos a formato numérico para crear dos modelos de ML y hacer que éstos estudien la mayor parte de los datos en cuestión para que, con el resto de los datos, se pueda testear la confiabilidad de dichos modelos, los resultados de los análisis, y por qué tomaron las decisiones que tomaron a la hora de decidir si un cliente supuestamente iba a irse o no.

## El repositorio

El repositorio consiste en tres archivos:
* El presente `README.md`.
* Un archivo en formato `CSV` en el que están almacenados los datos con los que trabaja el notebook.
* El programa, en formato `.ipynb`, que se ejecuta mediante Google Colab.

## Ejecución del programa

Para que el programa funcione, hay que contar con una cuenta de Gmail y acceso a Google Colab. Se debe abrir el Notebook abriendo el archivo `.ipynb` (una forma de abrirlo es contar con Google Drive, arrastrar el Notebook allí o a alguna carpeta del mismo —recomiendo que sea en la carpeta de nombre *Colab Notebooks*, ya que allí es donde se guardan por defecto los notebooks de Colab—, hacer clic derecho sobre el archivo, seleccionar *Abrir con*, y elegir *Google Colaboratory*). 

El archivo `datos_tratados.csv` (véase este mismo repositorio) debe cargarse en la carpeta de *Archivos*, al costado izquierdo de la pantalla de la página de Google Colab, a la misma altura que la carpeta `sample_data` (NO dentro de ella); es decir, en la ruta `/content/datos_tratados.csv` dentro de la máquina virtual Linux generada en el notebook de Colab.

### Librerías utilizadas
Al principio del código, se importarán las librerías necesarias para el proyecto:
* **Pandas** para la manipulación y análisis del DataFrame.
* **Warnings** para desactivar las advertencias que aparezcan como consecuencia de la ejecución del programa.
* **Matplotlib** (y Pyplot más en particular) y **Seaborn** para el procesamiento de datos e impresión de gráficos.
* **Sci-Kit Learn** para el encoding y transformaciones de las columnas (a valores numéricos), estandarizar los datos, dividir el DataFrame en conjuntos de entrenamiento y prueba, generar los modelos, entrenarlos, testearlos, y obtener las métricas de evaluación.

## El programa y Análisis de Datos

El programa trabaja con el mismo DataFrame del challenge anterior (Telecom X LATAM - Parte 1), con los datos ya normalizados y las columnas en español. Lo primero que hace es importar el CSV y realizar una limpieza profunda:
1. Se eliminan variables ruidosas y redundantes (como el ID del cliente y las cuentas diarias).
2. Se analizan y transforman las variables categóricas ('object') a formato numérico utilizando técnicas de mapeo y *One-Hot Encoding* (variables dummy).
3. Se realiza un análisis de **multicolinealidad** mediante mapas de calor (correlación de Pearson) y Factor de Inflación de la Varianza (VIF), decidiendo eliminar variables altamente correlacionadas entre sí (como el *Cobro total*, y redundancias en los tipos de servicio de internet y telefonía) para garantizar la estabilidad matemática de los modelos.

## Modelado de Machine Learning

Una vez estructurado el conjunto de datos limpio (`X`) y aislada la variable objetivo (`y` - Cancelación del servicio), se dividieron los registros utilizando un split de 80% para entrenamiento y 20% para pruebas, aplicando estratificación para mantener la proporción de las clases.

Se entrenaron y compararon dos modelos predictivos:
* **Regresión Logística:** Un modelo lineal enfocado en la explicabilidad y el cálculo de probabilidades.
* **Random Forest Classifier:** un modelo de ensamble basado en árboles de decisión capaz de capturar relaciones no lineales complejas.

## Resultados y Conclusiones

Tras evaluar ambos modelos mediante Matrices de Confusión y Reportes de Clasificación, se determinó que **la Regresión Logística es el modelo óptimo para este problema de negocio.** Aunque el Random Forest obtuvo una exactitud (*accuracy*) marginalmente superior, la Regresión Logística alcanzó un **Recall (Sensibilidad) del 79%** a la hora de identificar la clase positiva (clientes que cancelan), frente a un deficiente 46% del Random Forest. En este contexto, es preferible lidiar con falsos positivos que dejar escapar a clientes reales a punto de darse de baja.

**Hallazgos de Negocio (Feature Importance):**
El análisis de los coeficientes reveló que la fuga de clientes no es aleatoria. Los principales factores ("drivers") que impulsan las cancelaciones son:
1. El **alto costo mensual** de los servicios, fuertemente vinculado a la tenencia de Fibra Óptica.
2. Los **contratos de mes a mes**, que facilitan la salida rápida del cliente.
3. La carencia de servicios de valor agregado, específicamente la **falta de Soporte Técnico y Seguridad Online**. 

Se recomienda a la empresa revisar la infraestructura técnica de la fibra óptica, y considerar estrategias de retención enfocadas en bonificar servicios de seguridad y soporte para atar el valor percibido a las facturas de alto costo.