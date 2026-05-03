---

# Executive Summary
## Optimización de Captación para Universidades Privadas
### Modelización de Variables Categóricas — TSDS 2025/2026

---

### 1. El problema

¿Puede un modelo predictivo identificar qué perfil de estudiantes universitario tiene mayor probabilidad de cada destino profesional (empleo indefinido, temporal, emprendimiento o precariedad) a 5 años de la graduación, y usar ese conocimiento para personalizar campañas de captación de una universidad privada?

---

### 2. Datos y metodología

- **Dataset:** EILU\_GRAD\_2019 (INE) — 32.000 estudiantes de grado españoles.
- **Variable objetivo:** `SIT_PRO` reagrupada en 4 clases (Asalariado Indefinido 49,2 %, Temporal 23,9 %, Becario/Sin Empleo 19,1 %, Autónomo/Empresario 7,9 %)
- **Predictores:** rama de conocimiento, tipo de universidad, nivel educativo de los padres, sexo, movilidad interprovincial
- **Modelos evaluados:** Regresión Logística Multinomial (base), Árbol de Decisión, Random Forest, XGBoost, LightGBM, RF + SMOTE
- **Métrica principal:** F1-macro

---

### 3. Principales hallazgos

- **La rama de conocimiento es el predictor más potente**: Ingeniería y Salud predicen empleo indefinido con fuerza; en Artes la probabilidad de precariedad se duplica respecto a la media.
- **La universidad privada se asocia con +7 pp de empleo indefinido y +3,3 pp de emprendimiento** en ramas de sociales, incluso controlando por origen familiar (57,3 % vs 49,2 % en indefinido).
- **La movilidad interprovincial es el predictor conductual más accionable** : los estudiantes con movilidad muestran sistemáticamente mayor probabilidad de empleo indefinido.
- **El capital socioeconómico familiar es un confounder relevante**: la interacción T\_UNIV × RAMA es estadísticamente significativa; parte del efecto aparente del tipo de universidad se explica por el nivel educativo de los padres.

---

### 4. Modelo recomendado

**Random Forest + SMOTE**
Supera al modelo base en F1-macro sin señales de overfitting significativas. El sobremuestreo SMOTE equilibra la clase minoritaria Autónomo/Empresario sin introducir data leakage.

---

### 5. Implicación accionable

**Recomendación para el equipo de marketing:** segmentar la inversión publicitaria por RAMA antes que por cualquier otra variable. En ingeniería, el mensaje debe enfatizar la tasa de empleo indefinido. En sociales, el diferencial de emprendimiento de la universidad privada es el argumento de mayor impacto. Incorporar la predisposición a la movilidad geográfica como criterio de cualificación de leads.

---

### 6. Limitaciones

- Los datos son antiguos y el mercado laboral cambia constantemente.
- El modelo es correlacional: no podemos atribuir causalidad al tipo de universidad sin un diseño cuasiexperimental 

---
