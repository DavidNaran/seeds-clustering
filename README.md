# Clustering de semillas de trigo

[Español](#español) · [English](#english)

<p align="center">
  <img src="docs/resultados.png" alt="Las 210 semillas proyectadas a dos componentes principales, coloreadas por variedad real y por los grupos de cada técnica. K-Means con k = 2: homogeneidad 0,48 y completitud 0,79. Clustering jerárquico (Ward) con 3 grupos: 0,71 y 0,71. DBSCAN con eps = 0,5: 4 grupos y 24 puntos de ruido, 0,70 y 0,54. / The 210 seeds projected onto two principal components, coloured by actual variety and by the clusters of each technique. K-Means with k = 2: homogeneity 0.48, completeness 0.79. Hierarchical clustering (Ward) with 3 clusters: 0.71 and 0.71. DBSCAN with eps = 0.5: 4 clusters and 24 noise points, 0.70 and 0.54." width="100%">
</p>

## Español

### En resumen

- **Qué:** una comparación de tres técnicas de clustering (K-Means, clustering jerárquico y DBSCAN) sobre 210 semillas de trigo. La pregunta es si los grupos que aparecen sin usar etiquetas coinciden con las tres variedades reales.
- **Contexto:** práctica de Aprendizaje Automático del Grado en Ingeniería Informática (UC3M, curso 2024-2025), hecha en pareja con Alex Rodríguez Carbonero.
- **Resultado:** el clustering jerárquico con enlace Ward y 3 grupos es el que mejor recupera las variedades (homogeneidad 0,71 y completitud 0,71). K-Means obtiene la mejor silueta con k = 2, pero ese agrupamiento une dos variedades en un mismo grupo.
- **Tecnologías:** Python, scikit-learn, SciPy, pandas, Matplotlib y seaborn.

### Datos

Hay 210 semillas, 70 de cada una de tres variedades de trigo, descritas por siete medidas geométricas del grano: área, perímetro, compacidad, longitud, anchura, coeficiente de asimetría y longitud del surco. No hay valores faltantes. La columna `clase` (la variedad) no se usa para agrupar, solo para evaluar el resultado al final.

Es el conjunto [*Seeds*](https://doi.org/10.24432/C5H30K) del repositorio UCI (Charytanowicz et al., 2010; licencia CC BY 4.0), con los nombres de las columnas en español.

### Cómo se hizo

1. **Escalado y reducción a 2D.** Se comparan `MinMaxScaler`, `RobustScaler` y `StandardScaler`, cada uno seguido de un análisis de componentes principales (PCA) con dos componentes, dentro de un `Pipeline`. Se elige `StandardScaler`, que conserva el 89&nbsp;% de la varianza en esas dos componentes. Las tres técnicas trabajan sobre esa proyección.
2. **K-Means.** Se prueba k de 2 a 9 con el método del codo y el índice de silueta. La silueta es máxima con k = 2 (0,52), y ese es el valor que se usa.
3. **Clustering jerárquico.** Se dibuja el dendrograma con enlace Ward y se comparan cuatro enlaces (Ward, completo, promedio y simple) con 3 grupos. El enlace simple deja casi todas las semillas en un único grupo; se elige Ward.
4. **DBSCAN.** Se estima `eps` con la gráfica de distancias al quinto vecino y se recorre de 0,1 a 0,9 con `min_samples = 5`. La mejor silueta (0,33) sale con `eps = 0,5`: 4 grupos y 24 puntos marcados como ruido.
5. **Comparación con las variedades reales.** Para cada técnica se calcula la tabla de contingencia entre grupos y variedades, la homogeneidad y la completitud, y se interpretan los grupos con diagramas de caja de las siete variables.

### Resultados

| Técnica | Grupos | Homogeneidad | Completitud |
|---|---|---|---|
| K-Means (k = 2) | 2 | 0,480 | 0,794 |
| **Clustering jerárquico (Ward)** | **3** | **0,712** | **0,714** |
| DBSCAN (`eps = 0,5`) | 4 y 24 puntos de ruido | 0,699 | 0,544 |

La homogeneidad mide si cada grupo contiene una sola variedad, y la completitud, si cada variedad cae entera en un mismo grupo. Las dos van de 0 a 1.

- **K-Means** separa bien una variedad, pero junta las otras dos en un solo grupo: 60 de las 70 semillas de la variedad 1 acaban con las 70 de la variedad 3. Por eso su completitud es alta y su homogeneidad baja.
- **El clustering jerárquico** forma tres grupos que corresponden cada uno a una variedad: 56, 68 y 67 de las 70 semillas de cada una.
- **DBSCAN** forma grupos bastante puros, pero reparte las variedades en más grupos y deja 24 semillas sin asignar.

El criterio de la silueta, que no usa las etiquetas, prefiere dos grupos aunque haya tres variedades: dos de ellas se solapan en las medidas geométricas. En el notebook, K-Means solo se compara con las variedades usando k = 2.

### Cómo ejecutarlo

```bash
pip install -r requirements.txt
jupyter notebook ClusteringSemillas_Notebook_g9.ipynb
```

Las semillas aleatorias están fijadas. El notebook se ha vuelto a ejecutar con las versiones de `requirements.txt` y reproduce las mismas cifras.

### Estructura

```text
ClusteringSemillas_Notebook_g9.ipynb   Notebook con el análisis completo
semillas.csv                           Datos (210 semillas, 7 variables y la variedad)
docs/                                  Figura del README
```

### Autores

David Naranjo Mateos y Alex Rodríguez Carbonero. Aprendizaje Automático, Grado en Ingeniería Informática, Universidad Carlos III de Madrid, curso 2024-2025.

## English

### In short

- **What:** a comparison of three clustering techniques (K-Means, hierarchical clustering and DBSCAN) on 210 wheat seeds. The question is whether the clusters found without labels match the three actual varieties.
- **Context:** coursework for Machine Learning, Computer Science and Engineering degree (UC3M, 2024-2025), done in a pair with Alex Rodríguez Carbonero.
- **Result:** hierarchical clustering with Ward linkage and 3 clusters recovers the varieties best (homogeneity 0.71, completeness 0.71). K-Means gets its best silhouette at k = 2, but that clustering merges two varieties into one cluster.
- **Tech:** Python, scikit-learn, SciPy, pandas, Matplotlib and seaborn.

### Data

There are 210 seeds, 70 from each of three wheat varieties, described by seven geometric measurements of the kernel: area, perimeter, compactness, length, width, asymmetry coefficient and groove length. There are no missing values. The `clase` column (the variety) is not used for clustering, only to evaluate the result at the end.

It is the [*Seeds*](https://doi.org/10.24432/C5H30K) dataset from the UCI repository (Charytanowicz et al., 2010; CC BY 4.0 licence), with the column names in Spanish.

### Approach

1. **Scaling and reduction to 2D.** `MinMaxScaler`, `RobustScaler` and `StandardScaler` are compared, each followed by principal component analysis (PCA) with two components, inside a `Pipeline`. `StandardScaler` is chosen; it keeps 89% of the variance in those two components. All three techniques work on that projection.
2. **K-Means.** k is tried from 2 to 9 with the elbow method and the silhouette score. The silhouette peaks at k = 2 (0.52), which is the value used.
3. **Hierarchical clustering.** The Ward dendrogram is drawn and four linkages (Ward, complete, average and single) are compared with 3 clusters. Single linkage puts almost every seed in one cluster; Ward is chosen.
4. **DBSCAN.** `eps` is estimated from the distance-to-fifth-neighbour plot and swept from 0.1 to 0.9 with `min_samples = 5`. The best silhouette (0.33) comes at `eps = 0.5`: 4 clusters and 24 points labelled as noise.
5. **Comparison with the actual varieties.** For each technique the notebook computes the contingency table between clusters and varieties, homogeneity and completeness, and interprets the clusters with box plots of the seven variables.

### Results

| Technique | Clusters | Homogeneity | Completeness |
|---|---|---|---|
| K-Means (k = 2) | 2 | 0.480 | 0.794 |
| **Hierarchical clustering (Ward)** | **3** | **0.712** | **0.714** |
| DBSCAN (`eps = 0.5`) | 4 and 24 noise points | 0.699 | 0.544 |

Homogeneity measures whether each cluster holds a single variety, and completeness whether each variety falls entirely in one cluster. Both range from 0 to 1.

- **K-Means** separates one variety well but puts the other two in a single cluster: 60 of the 70 seeds of variety 1 end up with all 70 of variety 3. That is why its completeness is high and its homogeneity low.
- **Hierarchical clustering** forms three clusters that each correspond to one variety: 56, 68 and 67 of the 70 seeds of each.
- **DBSCAN** forms fairly pure clusters, but splits the varieties across more clusters and leaves 24 seeds unassigned.

The silhouette criterion, which does not use the labels, prefers two clusters even though there are three varieties: two of them overlap in the geometric measurements. In the notebook, K-Means is only compared with the varieties at k = 2.

### How to run it

```bash
pip install -r requirements.txt
jupyter notebook ClusteringSemillas_Notebook_g9.ipynb
```

Random seeds are fixed. The notebook has been re-run with the versions in `requirements.txt` and reproduces the same figures.

### Structure

```text
ClusteringSemillas_Notebook_g9.ipynb   Notebook with the full analysis
semillas.csv                           Data (210 seeds, 7 variables and the variety)
docs/                                  README figure
```

### Authors

David Naranjo Mateos and Alex Rodríguez Carbonero. Machine Learning, Computer Science and Engineering degree, Universidad Carlos III de Madrid, 2024-2025.
