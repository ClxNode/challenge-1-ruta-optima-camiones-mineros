# 🚛 Ruta Óptima de Camiones Mineros

**Challenge 1 — Búsqueda Inteligente y Optimización**

* **Curso:** Machine Learning
* **Unidad:** Fundamentos de IA y Búsqueda
* **Universidad:** Universidad Peruana Cayetano Heredia
* **Carrera:** Ingeniería Informática
* **Estudiante:** Juan Vidal Berrocal Ccapcha
* **Plataforma:** Google Colab

---

## 1. 🎯 Problema

### Contexto

En una mina a tajo abierto, los camiones transportan mineral desde las zonas de carga hacia las zonas de descarga. La elección de la ruta afecta directamente el tiempo de operación, el consumo de combustible y los riesgos asociados al terreno y a las condiciones de los tramos.

### Objetivo

Determinar la ruta óptima para un camión minero considerando:

* **Distancia:** Longitud total del trayecto.
* **Tiempo de recorrido:** Estimación en minutos según el terreno.
* **Consumo de combustible:** Índice de gasto energético.
* **Riesgo del tramo:** Peligrosidad según la estabilidad y curvas.
* **Pendiente del terreno:** Grado de inclinación acumulado.

> **Pregunta guía:**  
> *¿Cómo determinar una ruta óptima para un camión minero considerando diferentes factores operativos mediante estrategias de búsqueda, aprendizaje y optimización?*

---

## 2. 🗺️ Representación del Problema

La red de transporte se modela mediante un **grafo dirigido y ponderado**, donde los nodos representan intersecciones o zonas de la operación minera y las aristas corresponden a los tramos disponibles.

### Nodos Principales
* `Carga` *(Nodo Origen)*
* `Cruce_A`, `Cruce_B`, `Cruce_C`, `Cruce_D`, `Cruce_E` *(Nodos Intermedios)*
* `Descarga` *(Nodo Destino)*

### Atributos de los Tramos

- **Distancia (`d`):** Longitud del tramo expresada en kilómetros.
- **Pendiente (`p`):** Porcentaje de inclinación de la vía.
- **Riesgo (`r`):** Coeficiente de peligrosidad asociado al tramo, con valores entre `0` y `1`.
- **Condiciones de la vía:** Características de la superficie que pueden afectar el recorrido del camión.

---

## 3. 🔄 Espacio de Estados

El espacio de estados representa las posibles posiciones que ocupa el camión durante su trayecto.

| Elemento | Descripción / Valor |
| :--- | :--- |
| **Estado Inicial** | `Carga` |
| **Estados Intermedios** | `Cruce_A`, `Cruce_B`, `Cruce_C`, `Cruce_D`, `Cruce_E` |
| **Estado Objetivo** | `Descarga` |
| **Acción** | Desplazamiento hacia un nodo adyacente conectado |
| **Transición** | Cambio de estado de un nodo a otro tras recorrer el tramo |

**Ejemplo de ruta explorada:**  
`Carga` $\rightarrow$ `Cruce_A` $\rightarrow$ `Cruce_C` $\rightarrow$ `Descarga`

---

## 4. 🔎 Estrategias de Búsqueda

Se implementaron y evaluaron cuatro estrategias fundamentales para recorrer el espacio de estados:

| Algoritmo | Tipo de Búsqueda | Descripción / Propiedad |
| :--- | :--- | :--- |
| **BFS** | No Informada | Explora el grafo por niveles completos. Garantiza la ruta con menor número de aristas. |
| **DFS** | No Informada | Explora en profundidad cada rama antes de retroceder (*backtracking*). |
| **Fuerza Bruta** | Exhaustiva | Evalúa la totalidad de rutas posibles en el grafo para hallar el mínimo global. |
| **A\*** | Informada | Utiliza una función de evaluación $f(n) = g(n) + h(n)$ para guiar la búsqueda. |

### Resultados Iniciales por Algoritmo

* **BFS:** `Carga` → `Cruce_A` → `Cruce_C` → `Descarga` *(Optimiza saltos, no costos)*
* **DFS:** `Carga` → `Cruce_B` → `Cruce_D` → `Descarga`
* **Fuerza Bruta:** `Carga` → `Cruce_B` → `Cruce_D` → `Descarga` | **Costo:** `18.96`
* **A\* Inicial:** `Carga` → `Cruce_B` → `Cruce_D` → `Descarga` | **Costo:** `18.96`

---

## 5. 🤖 Incorporación de Datos Históricos

Se integraron registros de operaciones pasadas para ajustar dinámicamente los tiempos de recorrido en función de la inclinación y el riesgo de cada tramo.

### Estimación del tiempo

```text
Tiempo estimado = Tiempo base × (1 + Pendiente / 100) × (1 + Riesgo)
```

---

## 6. 💰 Función de Costo

El costo operativo global combina de forma ponderada tres factores clave:

- ⏱️ **Tiempo de recorrido**
- ⛽ **Consumo de combustible**
- ⚠️ **Riesgo operativo**

### Función de costo

```text
Costo = (w_t × Tiempo) + (w_c × Combustible) + (w_r × Riesgo)

```

## 7. 🧠 Optimización de Pesos mediante Hill Climbing

Para encontrar la combinación óptima de ponderaciones ($w_t, w_c, w_r$) que minimice el costo general sin sesgar la búsqueda hacia una sola variable, se aplicó un algoritmo de **Hill Climbing** (búsqueda local).

```text
  [ Pesos Iniciales ]
  wt = 0.500 | wc = 0.300 | wr = 0.200
               │
               ▼
   Iteraciones de ajuste local
   (Generación de perturbaciones Δw)
               │
               ▼
  [ Pesos Optimizados ]
  wt = 0.106 | wc = 0.160 | wr = 0.735
```

---

## 8. ⭐ A* Optimizado y Ruta Final

Con la función de costo calibrada por el algoritmo de Hill Climbing, se ejecutó nuevamente la búsqueda informada A*.

* **Ruta Óptima Obtenida:** `Carga` $\rightarrow$ `Cruce_A` $\rightarrow$ `Cruce_C` $\rightarrow$ `Descarga`
* **Costo Total Optimizado:** `5.10`

---

## 9. 📊 Resultados y Métricas Comparativas

### Métricas de la Ruta Óptima Final

| Métrica Operativa | Valor Obtenido |
| :--- | :---: |
| **Número de Tramos** | `3` |
| **Distancia Total** | `9.52 km` |
| **Tiempo Estimado** | `29.4 min` |
| **Índice de Combustible** | `10.01` |
| **Riesgo Acumulado** | `0.53` |
| **Costo Ponderado Optimizado** | **`5.10`** |

### Comparativa entre Estrategias

| Algoritmo / Fase | Ruta Resultante | Costo Ponderado |
| :--- | :--- | :---: |
| **Fuerza Bruta** | `Carga` ➔ `Cruce_B` ➔ `Cruce_D` ➔ `Descarga` | `18.96` |
| **A\* Inicial** | `Carga` ➔ `Cruce_B` ➔ `Cruce_D` ➔ `Descarga` | `18.96` |
| **A\* Optimizado** | `Carga` ➔ `Cruce_A` ➔ `Cruce_C` ➔ `Descarga` | **`5.10`** |
---

## 10. 🔄 Pipeline del Sistema

```text
┌──────────────────────────────────────────────────────────┐
│                  Definición del Problema                  │
└────────────────────────────┬─────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────┐
│              Representación como Grafo y              │
│                  Espacio de Estados                      │
└────────────────────────────┬─────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────┐
│      Búsquedas Iniciales (BFS / DFS / Fuerza Bruta)      │
└────────────────────────────┬─────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────┐
│             A* Inicial + Datos Históricos                │
└────────────────────────────┬─────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────┐
│        Optimización de Pesos vía Hill Climbing           │
└────────────────────────────┬─────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────┐
│         A* Optimizado -> Ruta y Costo Final              │
└────────────────────────────┴─────────────────────────────┘
```

---
## 11. 🛠️ Herramientas y tecnologías

### Lenguaje y entorno
<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=white" alt="Google Colab"/>
</p>

### Librerías
<p align="center">
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas"/>
  <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" alt="NumPy"/>
  <img src="https://img.shields.io/badge/Scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" alt="Scikit-learn"/>
  <img src="https://img.shields.io/badge/NetworkX-2C3E50?style=for-the-badge" alt="NetworkX"/>
  <img src="https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=matplotlib&logoColor=white" alt="Matplotlib"/>
</p>

### Algoritmos utilizados
* **BFS** — Breadth-First Search
* **DFS** — Depth-First Search
* **Fuerza Bruta**
* **A\*** — Búsqueda informada
* **Hill Climbing** — Búsqueda local
