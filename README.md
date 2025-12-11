# Crimen_Per_Capita


# 🇨🇱 Sistema de Análisis Delictual (Chile)

Este proyecto implementa un sistema de análisis de datos criminales utilizando una **arquitectura de base de datos híbrida (Redis + MongoDB)**. Permite la ingesta masiva de datos, consultas interactivas por consola y visualización gráfica de tasas de criminalidad, cruzando información de delitos con proyecciones demográficas.

## 🚀 Características Principales

* **Arquitectura Híbrida:**
    * **MongoDB:** Almacena el historial transaccional de delitos (Big Data).
    * **Redis:** Actúa como caché de alto rendimiento para metadatos geográficos (CUTs) y proyecciones de población.
* **ETL Automatizado:** Script de carga que limpia las bases de datos, cruza fuentes de información (CSV) y normaliza los datos para su uso.
* **CLI Interactivo:** Menú de consola para consultar tasas de criminalidad por comuna/región y generar rankings de peligrosidad en tiempo real.
* **Visualización de Datos:** Generación de gráficos comparativos (Matplotlib) para analizar la correlación entre volumen de delitos y población (Tasa x 100k hab).

## 📋 Requisitos Previos

Para ejecutar este proyecto, necesitas tener instalado y en ejecución:

1.  **Python 3.8+**
2.  **Redis Server** (Puerto por defecto: `6379`).
3.  **MongoDB Server** (Puerto por defecto: `27017`).

### Archivos de Datos
El sistema requiere dos archivos CSV en la raíz del proyecto (no incluidos en el repositorio por razones de privacidad):
* `poblacion.csv`: Proyecciones del INE (columnas requeridas: `cut_comuna`, `población`, `año`).
* `output.csv`: Base de datos de delitos (columnas requeridas: `fecha`, `delito`, `delito_n`, `cut_comuna`, `comuna`, `region`).

Recuerda que:

- **`output.csv`** contiene los datos de delitos en Chile.
- **`poblacion.csv`** contiene la población por comuna.

Para asegurar el funcionamiento adecuado, coloca los siguientes scripts en la **misma carpeta** que ambos archivos CSV:

- `cargar_final_2025.py`
- `app_interactiva.py`
- `analisis_final.py`

---

## ⚠️ Indicaciones Importantes

### 1. Configuración de ruta en `cargar_final_2025.py`
Dentro del script existe la variable:

```python
path_archivo = 'ruta/a/poblacion.csv'
```
### 2. Uso de `app_interactiva.py`

> **⚠️ Nota Importante:** La búsqueda es sensible a la escritura exacta.

Para obtener resultados, debes ingresar el nombre de la comuna respetando mayúsculas, tildes y sin espacios adicionales.

* ✅ **Correcto:** `Temuco`
* ❌ **Incorrecto:** `teumco` (errores de tipeo)
* ❌ **Incorrecto:** `Temuco ` (espacios al final)

Si la entrada no coincide exactamente, el programa no podrá procesar la solicitud.

---

### 3. Visualización en `analisis_final.py`

Este script genera un gráfico estadístico diseñado para comparar la criminalidad entre zonas de forma clara. La visualización incluye:

* Las **5 regiones más peligrosas** de Chile.
* La **tasa de delitos** calculada por cada 100.000 habitantes.

## 🛠️ Instalación y Configuración

1.  **Clonar el repositorio:**
    ```bash
    git clone [https://github.com/tu-usuario/crimen-chile-db.git](https://github.com/tu-usuario/crimen-chile-db.git)
    cd crimen-chile-db
    ```

2.  **Instalar dependencias:**
    Se recomienda utilizar un entorno virtual.
    ```bash
    pip install pandas redis pymongo matplotlib numpy
    ```

3.  **⚠️ Configuración de Rutas:**
    Antes de correr el sistema, **debes editar el archivo `cargar_datos.py`**.
    Busca la variable `path_archivo` (aprox. línea 35) y actualízala con la ruta local donde tengas tu archivo `poblacion.csv`:

    ```python
    # En cargar_datos.py
    path_archivo = 'ruta/local/a/tu/archivo/poblacion.csv'
    ```

## ▶️ Guía de Uso

El flujo de trabajo del sistema consta de tres pasos secuenciales:

### 1. Carga de Datos (ETL)
Ejecuta este script primero para poblar Redis y MongoDB.
```bash
python cargar_datos.py
```
### 2. Análisis Gráfico
Genera un gráfico de doble eje (Barras y Líneas) mostrando el Top 5 de regiones más peligrosas.

```bash
python analisis_datos.py
```
El gráfico comparará Población vs. Cantidad de Delitos y mostrará la Tasa de Criminalidad.

### 3. Consultas Interactivas (Menú)
Accede a la interfaz de línea de comandos para realizar búsquedas específicas.

```bash
python menu_interactivo.py
```
