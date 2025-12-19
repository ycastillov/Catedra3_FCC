# Optimización de Localización de Instalaciones (UFLP) con VQE

Este repositorio contiene la implementación del algoritmo **Variational Quantum Eigensolver (VQE)** para resolver el **Problema de Localización de Instalaciones sin Restricciones de Capacidad (UFLP)**.

Este proyecto fue desarrollado como parte de la asignatura **Fundamentos de la Computación Cuántica (2025-II)** de la Universidad Católica del Norte.

## 👥 Equipo de Trabajo

* **Yamir Castillo**
* **Juan Almonte**
* **Nelson Guaico**

---

## 🎯 Descripción del Problema

El problema UFLP consiste en seleccionar la ubicación óptima de bodegas y asignar clientes a ellas para minimizar el costo total, compuesto por:

1. **Costos de Instalación ($k_i$):** Costo fijo por abrir una bodega.
2. **Costos de Transporte ($c_{ij}$):** Costo de atender a un cliente desde una bodega específica.

Utilizamos computación cuántica variacional (VQE) para transformar este problema de optimización con restricciones en un modelo **QUBO** y encontrar su estado de mínima energía (solución óptima).

---

## 🛠️ Tecnologías Utilizadas

El proyecto fue implementado en **Python 3.12** utilizando las librerías modernas de Qiskit (v1.0+):

* **`qiskit`**: SDK principal para computación cuántica.
* **`qiskit-algorithms`**: Implementación de algoritmos variacionales (`SamplingVQE`).
* **`qiskit-optimization`**: Modelado de problemas cuadráticos y conversión automática a Hamiltonianos de Ising.
* **`qiskit-aer`**: Simulador de alto rendimiento.

### Librerías Clave y Justificación

* **`SamplingVQE`** + **`StatevectorSampler`**: Seleccionados en lugar del estimador estándar para permitir el muestreo de *bitstrings* (soluciones discretas 0/1) necesarias para interpretar la ubicación de bodegas.
* **`EfficientSU2`**: *Ansatz* "Hardware Efficient" utilizado para minimizar la profundidad del circuito.
* **`COBYLA`**: Optimizador clásico libre de gradientes, ideal para simulaciones rápidas.

---

## 🚀 Instalación y Ejecución

1. **Clonar el repositorio:**
```bash
git clone https://github.com/ycastillov/Catedra3_FCC.git
cd Catedra3_FCC

```


2. **Instalar dependencias:**
```bash
pip install qiskit qiskit-algorithms qiskit-optimization qiskit-aer numpy

```


3. **Ejecutar el Notebook:**
Abre el archivo `vqe.ipynb` en Jupyter Notebook o VS Code y ejecuta todas las celdas.

---

## 📊 Casos de Estudio y Resultados

Para validar la robustez del algoritmo, se analizaron dos instancias de complejidad creciente utilizando una estrategia de **Multi-Start** (reinicios múltiples) para evitar mínimos locales.

### 🔹 Caso 1: Instancia Base (2 Bodegas, 2 Clientes)

Escenario simple para validar la lógica.

* **Bodega 0:** Costo bajo (10). Transporte barato a Cliente 0.
* **Bodega 1:** Costo alto (20). Transporte barato a Cliente 1.

**Resultado VQE:**

* **Costo Mínimo:** `16.0`
* **Decisión:** `['x_0', 'y_0_0', 'y_0_1']`
* **Interpretación:** Se abre solo la **Bodega 0** (barata) y atiende a ambos clientes. El algoritmo aprendió correctamente que el ahorro en instalación compensa el mayor costo de transporte al Cliente 1.

### 🔹 Caso 2: Instancia Compleja (3 Bodegas, 4 Clientes)

Escenario diseñado con "trampa" lógica (Mínimo Local vs Global).

* **Bodegas 0 y 1:** Baratas (15), ubicadas en extremos opuestos (Norte/Sur).
* **Bodega 2:** Central pero muy cara (40).

**Resultado VQE:**

* **Costo Mínimo Global:** `34.0`
* **Decisión:** `['x_0', 'x_1', 'y_0_0', 'y_0_1', 'y_1_2', 'y_1_3']`
* **Análisis:** El algoritmo **evitó la trampa** de abrir la bodega central. En su lugar, decidió abrir **dos bodegas** (0 y 1) para minimizar los costos de transporte locales, demostrando capacidad para encontrar soluciones combinatorias complejas.

---

## 📄 Estructura del Repositorio

```text
├── vqe.ipynb       # Código principal (Jupyter Notebook) con la implementación completa.
└── README.md       # Documentación del proyecto.

```

---

*Proyecto realizado para la Cátedra 3 de Fundamentos de la Computación Cuántica - UCN 2025.*
