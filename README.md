# 💧 Datathon SUNASS 2026 - Reto Operacional: Solución Analítica y Optimización de Redes de Agua (Región Lima)

Álvaro Fabricio Villanueva Kobayashi


## 📌 Sobre Mí y el Enfoque del Proyecto

Soy Bachiller en Ingeniería Industrial, especializado en Data Science y Analítica de Datos. Mi enfoque profesional se centra en la automatización de procesos, la inteligencia de negocios y la eliminación de cuellos de botella corporativos mediante el desarrollo de algoritmos en Python y la implementación de modelos de Machine Learning.

En este repositorio presento mi solución integral para el **Reto Operacional de la Datathon SUNASS 2026**, enfocado en la problemática hídrica de la región Lima. Como científico de datos y auditor de calidad (QA), diseñé una arquitectura que no solo resuelve la logística de rutas, sino que garantiza el rigor, la integridad y la máxima fiabilidad de la información subyacente.

## 🏢 Contexto del Negocio

La Empresa Prestadora de Servicios (EPS) de la región Lima gestiona una infraestructura crítica con más de 300,000 conexiones activas. El principal desafío para optimizar sus operaciones se divide en tres frentes:

1. **Baja calidad de datos:** Presencia de duplicidad de registros, *outliers* de consumo y coordenadas geográficas gravemente erróneas.
2. **Falta de consolidación:** Ausencia de un perfil operativo claro que permita priorizar los sectores de la red según su criticidad.
3. **Logística ineficiente:** Necesidad de trazar rutas diarias óptimas para que las cuadrillas de reparación solucionen fugas y roturas minimizando sus desplazamientos.

## 🏗️ Arquitectura de la Solución y Metodología QA

Mi *pipeline* técnico se ejecuta en cinco fases clave. He puesto un énfasis exhaustivo en la limpieza de datos (QA), asumiendo que ningún modelo predictivo es válido si se alimenta de bases inconsistentes.

### Fase 1: Configuración e Ingesta

El entorno está preparado para escalar dinámicamente en Google Colab o en ejecución local, cargando los archivos brutos `.xlsx` de caudales, roturas y redes de distribución.

### Fase 2: Auditoría QA y Data Cleansing

Implementé algoritmos precisos de saneamiento para corregir el daño estructural en las bases de datos de la EPS:

* **Normalización Textual:** Apliqué expresiones regulares (`re`) para unificar 2534 variantes tipográficas, consolidándolas en exactamente 400 nombres de sectores únicos.
* **Deduplicación Estricta:** Construí una llave compuesta (`CONEXION`, `CATEGORIA`, `fecha_inicio`) que me permitió localizar y eliminar 303 sobreregistros operativos.
* **Corrección Espacial:** Programé una función de detección vectorial que corrigió 5554 inconsistencias geográficas donde las coordenadas de latitud y longitud habían sido invertidas.
* **Tratamiento de Outliers:** Utilicé una regla estadística basada en el límite superior del rango intercuartílico ($Q3 + 1.5 \times IQR$) para detectar y ajustar 104 valores de caudales anómalos correspondientes a 50 sectores distintos.

### Fase 3: Consolidación e Ingeniería de Características (Feature Engineering)

* Calculé los tiempos reales de acción técnica, hallando un tiempo promedio de reparación de 59.77 horas para roturas matrices y 26.03 horas para fugas secundarias.
* Diseñé ratios estandarizados libres de sesgo de tamaño. Encontré una fuerte correlación lineal de Pearson ($\rho = 0.885$) entre mis indicadores clave de "Roturas por km de red" y "Fugas por cada 1000 conexiones".

### Fase 4: Segmentación Estratégica (Clustering)

* Para enfocar los recursos de atención, entrené un modelo de Machine Learning no supervisado (**K-Means**) estandarizado previamente con `StandardScaler`.
* Al contrastar el método del codo (Inertia) con el *silhouette score*, comprobé matemáticamente que el número óptimo de clústers es $k=2$, logrando un excelente score de validación de $0.789$.

### Fase 5: Optimización Logística de Cuadrillas (Algoritmo TSP)

Abordé el despacho diario de las cuadrillas operativas (C1, C2 y C3) tratándolo algorítmicamente como el **Problema del Agente Viajero (TSP)**.

* Combiné una heurística de inicialización de **Vecino Más Cercano (Nearest Neighbor)** para hallar una ruta base rápida, seguida de una profunda optimización local usando el algoritmo **Two-Opt** para desentrelazar cruces ineficientes.
* El sistema devuelve diccionarios con la senda óptima diaria y las distancias euclidianas minimizadas por cada cuadrilla técnica.

## 💻 Stack Tecnológico Utilizado

* **Python 3.12.13:** Lenguaje principal de modelado.
* **Pandas & NumPy:** Orquestación, manejo vectorial y QA de datos.
* **Scikit-Learn:** Modelado `KMeans` y validación de métricas.
* **Matplotlib & Seaborn:** Visualización de clústers y renderizado geográfico de las sendas de cuadrillas.
* **OpenPyXL:** Exportación de resultados finales.

## 🚀 Instrucciones de Ejecución

Para la replicabilidad técnica del script:

1. Instale las librerías necesarias con el gestor de paquetes de Python.
2. Deposite los archivos crudos de caudales, roturas y redes en el directorio raíz `/content` (si usa Colab) o en el `cwd` (local).
3. Ejecute el notebook analítico completo `solucion_lima_ SCC07.ipynb` para generar los resultados tabulados por cuadrilla y día objetivo.
