# Optimización de captación para Universidades Privadas
### Modelización de Variables Categóricas — Pregunta B
**TSDS 2025/2026 · ESIC University**  
**Grupo:** Fernando Marañón, Javier Marín, Juan Pérez, Álvaro Sánchez

---
## Pregunta de investigación

> **¿Qué perfil de estudiante universitario (rama, tipo de universidad, origen socioeconómico, movilidad y sexo) se asocia con cada uno de los cuatro destinos profesionales principales, cuatro años después de la graduación?**

---

## Variable objetivo

`SIT_PRO` reagrupada en **4 categorías**:

| Clase | Descripción | Frecuencia aproximada |
|---|---|---|
| `Asalariado_Indefinido` | Contrato indefinido | ~50 % |
| `Asalariado_Temporal` | Contrato temporal | ~30 % |
| `Becario_SinEmpleo` | Becario o sin empleo | ~15 % |
| `Autonomo_Empresario` | Autónomo, empresario o colaborador familiar | ~5 % |

---

## Cómo reproducir el análisis

Los notebooks deben ejecutarse **en orden**, desde la raíz del repositorio o desde la carpeta `notebooks/`:

```
1. notebooks/01_EDA.ipynb
2. notebooks/02_modelo_base.ipynb
3. notebooks/03_modelos_avanzados.ipynb
```

Cada notebook carga el dataset directamente desde `data/raw/`. El dataset procesado generado por el notebook 01 se guarda en `data/clean/` y puede reutilizarse en los siguientes pasos.

---

## Dataset

**EILU_GRAD_2019** — Encuesta de Inserción Laboral de Titulados Universitarios 2019 (INE).  
Ver instrucciones de descarga y descripción de columnas en `data/README.md`.
---

## Estructura del repositorio

```
mvc-proyecto/
├── README.md                          # Este archivo
├── data/
│   ├── README.md                      # Instrucciones de descarga y descripción del dataset
│   ├── raw/
│   │   └── EILU_GRAD_2019.csv         # Dataset original (colocar el archivo que nos descargamos)
│   └── clean/
│       └── EILU_GRAD_processed.csv    # Dataset procesado generado por 01_EDA.ipynb
├── notebooks/
│   ├── 01_EDA.ipynb                   # Fase 1: EDA, preprocesado, encoding
│   ├── 02_modelo_base.ipynb           # Fase 2: Regresión logística multinomial
│   ├── 03_modelos_avanzados.ipynb     # Fase 3: RF, XGBoost, LightGBM, comparativa
│ 
├── outputs/
│   └── figures/                       # Graficos exportados en PNG (generados automaticamente)
└── docs/
    └── executive_summary.md           # Resumen ejecutivo del análisis
```

---

## Variables del modelo

### Variables del enunciado

| Variable | Descripción | Tipo |
|---|---|---|
| `T_UNIV` | Tipo de universidad (pública/privada, presencial/distancia) | Nominal |
| `RAMA` | Rama de conocimiento | Nominal |
| `ESTUDIOS_PADRE` | Nivel formativo del padre | Ordinal |
| `ESTUDIOS_MADRE` | Nivel formativo de la madre | Ordinal |
| `MOV_IN` | Movilidad interprovincial | Binaria |
| `SEXO` | Sexo del egresado | Binaria |
| `EDAD` | Tramo de edad al graduar | Condiciones de entrada al mercado laboral distintas por cohorte de edad |
| `DISCA` | Discapacidad reconocida | Acceso a cupos y programas específicos que distorsionan la distribución del target |
| `SAT1` | Satisfacción con la formación recibida | Proxy de calidad percibida del centro; señalización ante empleadores |
| `SAT2` | Satisfacción con la utilidad para el empleo | Ligada directamente a la empleabilidad percibida; alta correlación esperada con el target |
| `TIC` | Competencias digitales autodeclaradas | Capital humano complementario relevante en sectores de alta demanda |

---

## Decisiones de modelización

### class_weight='balanced'
El modelo de regresión logística base (y su versión en el comparativo) utiliza `class_weight='balanced'` para corregir el desbalanceo estructural del dataset. Sin este ajuste, el clasificador tiende a ignorar la clase `Autonomo_Empresario` (~5 %) y a saturar las predicciones hacia `Asalariado_Indefinido` (~50 %). Con `balanced`, cada clase recibe un peso inversamente proporcional a su frecuencia, mejorando el F1-macro a costa de un pequeño descenso de la accuracy global.

### Interacción T_UNIV × RAMA
La V de Cramér de `T_UNIV` con el target es baja (0.067), posiblemente porque su efecto varía por rama: la diferencia pública/privada puede ser pronunciada en salud e ingeniería y prácticamente nula en Artes.

---

## Principales hallazgos

1. **La rama de conocimiento es el predictor dominante**: ingeniería y salud predicen con fuerza el empleo indefinido; artes y ciencias muestran mayor precariedad.
2. **La universidad privada se asocia con mayor tasa de emprendimiento**, especialmente en ciencias sociales, aunque parte de este efecto se explica por el capital socioeconómico familiar.
3. **La movilidad interprovincial aumenta la probabilidad de empleo indefinido**, aunque la causalidad es difícil de establecer.
4. **El capital socioeconómico familiar actúa como confounder**: parte del efecto aparente de `T_UNIV` queda absorbida por `ESTUDIOS_PADRE` y `ESTUDIOS_MADRE`.


## Limitaciones
- Los datos son observacionales: no es posible establecer causalidad entre tipo de universidad y destino profesional sin diseño experimental.
- La cohorte corresponde a graduados de 2013/2014; el mercado laboral ha cambiado desde entonces.
