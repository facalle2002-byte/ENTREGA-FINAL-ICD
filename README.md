# ENTREGA-FINAL-ICD
Predicción del acceso adecuado a controles prenatales utilizando ENDES 2018–2023

Este proyecto analiza los factores que explican el acceso adecuado a controles prenatales en el Perú, utilizando microdatos de la **Encuesta Demográfica y de Salud Familiar (ENDES)** para los años **2018 a 2023**.
El análisis se basa en datos individuales de madres entrevistadas, permitiendo modelar la probabilidad de que reciban la atención prenatal mínima recomendada por el Ministerio de Salud.

Para ello, construimos un pipeline completo que incluye:

* Exploración de datos (EDA)
* Modelos base (Logit)**
* Modelos avanzados de clasificación (XGBoost, MLP)**
* Optimización de umbral y costo social FP/FN**
* Técnicas de balanceo (SMOTE)**
* Construcción de un DAG causal**
* Análisis de importancia de características**

 Fuente de Datos

Los microdatos provienen del repositorio público del INEI:

* **ENDES – Microdatos 2018 a 2023**
* Unidad de análisis: **madres entrevistadas** (módulo de salud materna)
* Base consolidada y limpiada:
  `final_df_limpio.csv` (incluida en este repositorio)

---

 **Variable Objetivo**

La variable dependiente es:

### **`prenatal_adecuado` (binaria)**

* **1** → La madre recibió el número mínimo recomendado de controles prenatales.
* **0** → No recibió controles suficientes.

---

## **Variables Utilizadas**

Del dataset original seleccionamos y construimos las siguientes variables clave:

### **Variables sociodemográficas**

* `edad` – Edad de la madre
* `edad_sq` – Edad al cuadrado
* `educacion_grupo` – Nivel educativo agrupado
* `nivel_riqueza` – Índice de riqueza (quintiles)
* `urbano` – Indicador de zona urbana/rural
* `SREGION` – Región natural (Sierra, Costa, Selva, Lima)

### **Variables derivadas (Feature Engineering)**

* `edad2` – Transformación continua alternativa de edad
* `edad_x_riqueza` – Interacción entre edad y riqueza
* `riqueza_x_urbano` – Interacción entre riqueza y zona urbana
* `riqueza_x_educ` – Interacción riqueza × educación
* `riesgo_edad` – Indicador si la madre está en rango de riesgo obstétrico por edad

Estas transformaciones permitieron capturar relaciones no lineales entre los factores socioeconómicos y el acceso prenatal.

---

## **Modelos Implementados**

1. **Logit Base (StatsModels)**
2. **Logit Mejorado (Sklearn + regularización + SMOTE + Pipeline)**
3. **Random Forest (versión diagnóstica)**
4. **XGBoost (tuned con cross-validation)**
5. **MLP – Red Neuronal Multicapa**

Cada modelo fue evaluado con métricas apropiadas para desbalance:

* **PR-AUC**
* **ROC-AUC**
* **Precision, Recall, F1-score**
* **Matriz de confusión**

Además, se calculó el **umbral óptimo** basado en costos sociales de errores tipo I y II.

---

## **Enfoque Causal – DAG**

Se construyó un **DAG causal** para identificar confounders y formalizar el mecanismo estructural:

* Educación, edad, región, riqueza y zona urbana afectan directa o indirectamente el acceso prenatal.
* Se identificaron rutas de confusión y posibles variables omitidas relevantes.

---

## **Principales Hallazgos**

* Las variables **región**, **educación** y **nivel de riqueza** son las que más peso tienen en la predicción (XGBoost).
* El fuerte desbalance requiere optimización del **umbral de decisión**, no solo accuracy.
* El umbral óptimo (~0.01) minimiza falsos negativos, crucial en política pública.
* El MLP confirma la robustez del XGBoost con **PR-AUC ≈ 0.92**.

---

## 🛠️ **Requisitos**

Para ejecutar el notebook:

```bash
pip install pandas numpy scikit-learn xgboost imbalanced-learn matplotlib seaborn networkx
```

---

## **Estructura del Repositorio**

```
ENTREGA-FINAL-ICD/
│── final_df_limpio.csv
│── TG4_GRUPO5_ICD.ipynb
│── README.md
```

---

## **Autores**

* Fabricio Calle
* Bianca Jimenez
* Jose Naira
* Aracely Lalupu
* Curso: **Introducción a Ciencia de Datos – UDEP 2025**

---

Si quieres, también te preparo una **versión corta** para la descripción del repositorio o una **versión académica tipo paper**.

