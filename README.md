ConnectaTel

Análisis exploratorio, limpieza y segmentación de clientes de **ConnectaTel**, una empresa de telecomunicaciones de Latinoamérica. Proyecto final del Sprint 7 (Python + pandas).

---

## 🎯 Objetivo

Evaluar el comportamiento de los clientes con información registrada hasta **2024** para:

- Construir un perfil estadístico de los usuarios (edad, plan, uso de llamadas y mensajes).
- Detectar comportamientos atípicos (valores extremos).
- Segmentar clientes por **edad** y por **nivel de uso**.
- Proponer ideas para mejorar los planes y las estrategias comerciales.

---

## 🗂️ Datasets

| Archivo | Filas | Descripción |
|---|---|---|
| `plans.csv` | 2 | Características de los planes (precio, minutos, mensajes y GB incluidos, costo por extra). |
| `users_latam.csv` | 4.000 | Clientes: edad, ciudad, fecha de registro, plan y fecha de baja (`churn_date`). |
| `usage.csv` | 40.000 | Eventos de uso en 2024: llamadas (`duration`) y mensajes (`length`). |

> Los archivos CSV no se incluyen en el repositorio. En el entorno del curso se leen desde `/datasets/`.

---

## 🧭 Etapas del análisis

1. **Carga y exploración**: lectura de los tres datasets, revisión de dimensiones y tipos de datos (`.shape`, `.info()`).
2. **Calidad de datos**
   - Valores nulos por columna (cantidad y porcentaje).
   - Sentinels y valores inválidos: `-999` en `age` y `"?"` en `city`.
   - Revisión de fechas: 40 usuarios con fecha de registro en 2026 (fuera del periodo 2022–2024).
3. **Limpieza**
   - `age`: `-999` reemplazado por la NaN.
   - `city`: `"?"` reemplazado por NaN.
   - `reg_date`: fechas fuera de rango marcadas como NaN.
   - `duration` y `length`: los nulos dependen de la columna `type` (las llamadas no tienen `length` y los mensajes no tienen `duration`), por lo que se mantienen como nulos.
4. **Métricas por usuario**: cantidad de mensajes, cantidad de llamadas y minutos totales de llamada, unidas al dataset de usuarios (`user_profile`).
5. **Visualización**: histogramas por plan y boxplots para detectar outliers (método IQR).
6. **Segmentación**
   - `grupo_edad`: Joven (< 30), Adulto (30–59), Adulto Mayor (≥ 60).
   - `grupo_uso`: Bajo (llamadas < 5 y mensajes < 5), Medio (ambos < 10), Alto (resto).
7. **Insight ejecutivo** para stakeholders.

---

## 📊 Principales resultados

**Distribución por plan:** Básico 64,9 % · Premium 35,1 %.

**Segmentos por edad**

<img width="485" height="352" alt="imagen" src="https://github.com/user-attachments/assets/85d7937a-9e7e-4983-8b3a-e26a11d4045b" />


**Segmentos por nivel de uso**

<img width="489" height="352" alt="imagen" src="https://github.com/user-attachments/assets/d6d53407-99d6-49a2-b98e-9c9270df2a01" />


**Consumo típico por usuario (mediana):** 5 mensajes, 4 llamadas y ~19,8 minutos de llamada.

**Observaciones**
- La mayoría de los clientes son adultos con consumo medio.
- Las variables de uso presentan sesgo a la derecha; los minutos de llamada tienen valores extremos (máx. ≈ 155 min) que conviene validar antes de decidir si son consumos reales o errores.

---

## ⚠️ Limitaciones y próximos pasos

- **Churn**: `churn_date` tiene 466 valores (11,65 % de los clientes) pero aún no se analiza la tasa de baja por plan, edad o nivel de uso.
- **Cruce de segmentos**: analizar `grupo_edad` × `grupo_uso` × `plan` en una sola tabla.
- **Outliers**: calcular los límites IQR (Q1 − 1,5·IQR y Q3 + 1,5·IQR) y contar cuántos usuarios los superan.
- **Usuarios sin registros de uso**: rellenar con 0 las métricas de uso para que no queden como nulos tras el `merge`.
- **Duplicados**: verificar `user_id` e `id` con `.duplicated()`.

---

## ▶️ Cómo ejecutar

### Opción A: Google Colab
1. Abre el notebook en Colab (**File → Open notebook → GitHub**) y pega la URL del repositorio.
2. Sube `plans.csv`, `users_latam.csv` y `usage.csv` al entorno de ejecución.
3. Ajusta las rutas de lectura si es necesario (por ejemplo, `pd.read_csv('users_latam.csv')`).
4. Ejecuta **Runtime → Restart and run all**.

### Opción B: Local
```bash
git clone https://github.com/jairoalexandergelves-bit/telecom-analysis.git
cd telecom-analysis
pip install pandas matplotlib seaborn jupyter
jupyter notebook
```
Coloca los CSV en la carpeta del proyecto y cambia `/datasets/` por la ruta local en la celda de carga.


## 📁 Estructura del repositorio

```
telecom-analysis/
├── Project-ConnectaTel-final.ipynb   # Notebook con todo el análisis
└── README.md
```

## 👤 Autor

**Jairo Alexander Gelves Ruiz** — Analista de datos junior
