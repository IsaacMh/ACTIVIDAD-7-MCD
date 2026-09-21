# Guía Ejecutiva y Práctica: Aprendizaje No Supervisado y Clustering
URL CODIGO COLAB: https://colab.research.google.com/drive/14z3jSMbZZiYYhou6bNtQJ-6Q8yeIbh8h?usp=sharing

PRESENTADOR POR: ISAAC JOEL MAMANI HUMPIRI

Este repositorio contiene un resumen ejecutivo completo, fundamentos teóricos, comparativas experimentales y código en Python/Google Colab sobre **Aprendizaje No Supervisado**, **Clustering**, **Reducción de Dimensiones** y **Detección de Anomalías**.



---

##  Tabla de Contenidos
1. [Fundamentos del Aprendizaje No Supervisado](#1-fundamentos-del-aprendizaje-no-supervisado)
2. [Taxonomía de Tareas y Algoritmos](#2-taxonomía-de-tareas-y-algoritmos)
3. [Métricas de Evaluación de Clustering](#3-métricas-de-evaluación-de-clustering)
4. [Experimentos Prácticos y Comparativa](#4-experimentos-prácticos-y-comparativa)
5. [Naturaleza Geométrica e Hiperparámetros Críticos](#5-naturaleza-geométrica-e-hiperparámetros-críticos)
6. [Cómo Ejecutar en Google Colab](#6-cómo-ejecutar-en-google-colab)

---

## 1. Fundamentos del Aprendizaje No Supervisado

El **aprendizaje no supervisado** es una rama del aprendizaje automático enfocada en entrenar modelos utilizando **datos sin etiquetar** (sin variable objetivo $y$). Su propósito principal es explorar de forma autónoma la estructura subyacente, relaciones, agrupaciones o patrones en las variables de entrada $X$.

A diferencia del aprendizaje supervisado —que busca realizar tareas predictivas guiadas por respuestas conocidas (como clasificar tumores en benignos o cancerosos)—, el enfoque no supervisado descubre la estructura inherente de la nube de observaciones con suposiciones mínimas preexistentes.

---

## 2. Taxonomía de Tareas y Algoritmos

###  A. Clustering (Agrupamiento)
Técnica descriptiva que busca particionar una nube de observaciones en subgrupos o **conglomerados** homogéneos.
* **Principio fundamental**: Minimizar la variación o distancia dentro de cada grupo (**distancia intracluster**) y maximizar la separación entre grupos distintos (**distancia intercluster**).
* **Tipos de Algoritmos**:
  * **Particionamiento**: *K-Means*, *PAM*, *CLARA*, *FANNY*. Dividen los datos en un número $K$ preespecificado minimizando la inercia interna.
  * **Jerárquicos**: Generan una secuencia anidada de grupos representada mediante un **dendrograma**. Pueden ser *Aglomerativos* (AGNES, de abajo hacia arriba) o *Divisivos* (DIANA, de arriba hacia abajo).
  * **Basados en Densidad y Grafo**: *DBSCAN* (encuentra clústeres de forma arbitraria por densidad continua y aísla ruido) y *Spectral Clustering* (evalúa la conectividad del grafo de afinidad no lineal).

###  B. Reducción de Dimensiones
Proceso para simplificar conjuntos de datos hiperdimensionales reduciendo el número de variables reteniendo la mayor variabilidad e información posible.
* **Pros**: Previene el sobreajuste (*overfitting*), elimina colinealidad, reduce costo computacional y permite visualización 2D/3D.
* **Técnicas**:
  * **Selección de características**: Elige un subconjunto de variables originales basándose en su poder predictivo.
  * **Extracción de características**: Transforma y combina variables mediante proyecciones *lineales* (PCA, LDA) o *no lineales* (t-SNE, Isomap).

###  C. Detección de Anomalías
Identificación de eventos u observaciones atípicas (*outliers*) que rompen el patrón normal del sistema (ej. fraude financiero, fallas de red, arritmias cardíacas en ECG).
* **Enfoques**: Umbral simple, Tasa de cambio (derivadas) y Monitoreo de forma.
* **Algoritmos**: *Isolation Forest* (referente, aísla anomalías en árboles), *Robust Covariance* (asume distribución normal) y *One-Class SVM*.

---

## 3. Métricas de Evaluación de Clustering

| Categoría | Métrica | Qué mide | Criterio de Calidad |
| :--- | :--- | :--- | :--- |
| **Interna** (Sin $y$) | **Coeficiente de Silueta** | Cohesión del clúster vs. separación con el vecino cercano (Rango: -1 a +1). | Cercano a **+1** es óptimo. |
| **Interna** (Sin $y$) | **Calinski-Harabasz** | Razón de varianza entre grupos frente a la varianza dentro del grupo (Pseudo-F). | **Mayor valor** es mejor. |
| **Interna** (Sin $y$) | **Davies-Bouldin** | Similitud promedio entre cada clúster y su más similar. | **Menor valor** (cercano a 0) es mejor. |
| **Interna** (Sin $y$) | **Inercia / WSS** | Suma de distancias al cuadrado de cada punto a su centroide asignado. | **Punto de codo** (*Elbow method*). |
| **Externa** (Con $y$) | **Adjusted Rand Index (ARI)** | Grado de coincidencia entre particiones y etiquetas reales, ajustado por azar. | **1.0** indica coincidencia perfecta. |
| **Externa** (Con $y$) | **Normalized Mutual Info (NMI)** | Cantidad de información compartida entre clústeres y clases reales. | **1.0** indica coincidencia perfecta. |
| **Externa** (Con $y$) | **Homogeneidad** | Verifica si cada clúster contiene únicamente observaciones de una sola clase. | **1.0** indica clústeres puros. |

---

## 4. Experimentos Prácticos y Comparativa

Evaluación de los tres modelos principales (**K-Means**, **DBSCAN** y **Spectral Clustering**) en tres escenarios representativos:

| Dataset | Modelo | Clústeres | Silueta ↑ | Calinski-Harabasz ↑ | Davies-Bouldin ↓ | ARI (Ext) ↑ | NMI (Ext) ↑ | Observaciones |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Iris** | K-Means ($K=3$) | 3 | 0.4599 | 241.90 | 0.8336 | 0.6201 | 0.6595 | Separa bien *Setosa*; corta frontera *Versicolor/Virginica*. |
| **Iris** | DBSCAN ($\epsilon=0.5$) | 2 | 0.3565 | 84.51 | 7.1241 | 0.4421 | 0.5114 | Junta las dos especies solapadas por continuidad de densidad. |
| **Iris** | Spectral ($K=3$) | 3 | **0.4630** | 236.89 | **0.8257** | **0.6451** | **0.6895** | Mejor rendimiento global al adaptar fronteras no lineales. |
| **Moons** | K-Means ($K=2$) | 2 | **0.4936** | **414.56** | **0.8056** | 0.4790 | 0.3819 | **Falla**: Corta las dos medias lunas por la mitad. |
| **Moons** | DBSCAN ($\epsilon=0.3$) | 2 | 0.3803 | 255.38 | 1.0252 | **1.0000** | **1.0000** | **Perfecto**: Sigue el camino denso continuo de cada luna. |
| **Moons** | Spectral ($K=2$) | 2 | 0.3803 | 255.38 | 1.0252 | **1.0000** | **1.0000** | **Perfecto**: Proyecta las lunas a un espacio linealmente separable. |
| **Circles** | K-Means ($K=2$) | 2 | **0.3532** | **174.94** | **1.1752** | -0.0033 | 0.0000 | **Falla total**: Asigna puntos como azar al trazar una línea recta. |
| **Circles** | DBSCAN ($\epsilon=0.35$) | 2 | 0.1098 | 0.01 | 163.27 | **1.0000** | **1.0000** | **Perfecto**: Separa el anillo exterior del círculo interno. |
| **Circles** | Spectral ($K=2$) | 2 | 0.1098 | 0.01 | 163.27 | **1.0000** | **1.0000** | **Perfecto**: Captura la geometría concéntrica vía Laplaciano. |

> ⚠️ **La Paradoja de la Evaluación Interna**: En estructuras concéntricas o no convexas (*Moons*, *Circles*), las métricas internas (Silueta, CH) evalúan mejor a K-Means porque este crea grupos compactos y redondos, a pesar de que la asignación real es totalmente errónea. Esto demuestra la importancia de usar métricas según la geometría del problema.

---

## 5. Naturaleza Geométrica e Hiperparámetros Críticos

###  Modelos Lineales
* **K-Means**: Trazado de hiperplanos lineales rígidos entre centroides.
  * **Hiperparámetros críticos**:
    * `n_clusters` ($K$): Cantidad de grupos a buscar (obligatorio). Seleccionar mediante Método del Codo o Silueta.
    * `init`: Inicialización de centroides. Usar `'k-means++'` para evitar óptimos locales deficientes.
* **PCA**: Proyección lineal en las direcciones de máxima varianza.

###  Modelos No Lineales
* **Spectral Clustering**: Construcción de matriz de afinidad y proyección Laplaciana no lineal.
  * **Hiperparámetros críticos**:
    * `n_clusters`: Número de clústeres.
    * `affinity`: Función de similitud (ej. `'nearest_neighbors'` o `'rbf'`).
* **DBSCAN**: Agrupamiento por continuidad de densidad en el espacio.
  * **Hiperparámetros críticos**:
    * `eps` ($\epsilon$): Radio máximo de vecindad. Ajustar analizando el codo en el gráfico de $K$-distancias.
    * `min_samples`: Mínimo de puntos requeridos para formar una región densa. *(Descubre $K$ automáticamente)*.

---

