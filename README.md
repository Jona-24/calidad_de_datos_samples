# Mis-recursos

Proyecto de análisis y limpieza de datos sobre recursos turísticos, museos y crimen en Madrid.

## Descripción

Este proyecto contiene análisis exploratorios y procesos de limpieza y normalización de tres conjuntos de datos principales:

- **Alojamientos Turísticos**: Datos sobre alojamientos y establecimientos turísticos
- **Museos**: Información de museos con validación y normalización de contacto
- **Crimen**: Datos de incidentes criminales con estandarización geográfica

## Estructura del Proyecto

```
Mis-recursos/
├── README.md                                    # Este archivo
├── venv/                                        # Entorno virtual Python (3.12.10)
├── data/                                        # Carpeta de datos
│   ├── alojamientos_turisticos.csv             # Datos originales de alojamientos
│   ├── alojamientos_turisticos_corregido.csv   # Datos de alojamientos limpios
│   ├── 201132-0-museos.csv                     # Datos originales de museos
│   ├── 201132-0-museos.json                    # Datos de museos en JSON
│   ├── museos_corregido.csv                    # Datos de museos limpios
│   ├── Act01_BD.*.json                         # Datos de entrada en JSON
│   ├── crimen_corregido.csv                    # Datos de crimen limpios
│   ├── data_act_01.csv                         # Datos de crimen originales
│   └── *.csv                                   # Otros archivos de datos
├── Analisis_turismo.ipynb                      # Notebook de análisis de alojamientos
├── Analisis Museos.ipynb                       # Notebook de análisis de museos
├── Analisis crimen.ipynb                       # Notebook de análisis de crimen
├── Recurso1.ipynb                              # Notebook de recursos iniciales
├── *.html                                      # Reportes de perfilado (ProfileReport)
└── my_database.db                              # Base de datos SQLite

```

## Notebooks

### 1. Analisis_turismo.ipynb

**Objetivo**: Limpiar y normalizar datos de alojamientos turísticos.

**Transformaciones realizadas**:
- **Localidad**: Eliminación de tildes y caracteres especiales, conservando espacios y alfanuméricos
- **Puerta**: Normalización de variantes (IZQ, IZDA, DCHA, etc.) a categorías estándar (IZQUIERDA, DERECHA, CENTRO, AMBAS, PRINCIPAL)
- **Vía Tipo**: Normalización de variantes de tipos de vía (CALLE, AVENIDA, CARRETERA, CARRERA)

**Columnas creadas durante el proceso**:
- `localidad_clean`: Localidad normalizada
- `puerta_norm`: Puerta normalizada
- `via_tipo_norm`: Tipo de vía normalizado
- `localidad_original`, `puerta_original`, `via_tipo_original`: Respaldos de valores originales

**Resultado**: `data/alojamientos_turisticos_corregido.csv`

### 2. Analisis Museos.ipynb

**Objetivo**: Validar y limpiar información de contacto de museos.

**Transformaciones realizadas**:
- **Email**: 
  - Validación mediante regex (mínimo 2 caracteres @ dominio + .com/.es/.org)
  - Relleno de nulos/vacíos con `contactanos@madrid.es`
  - Limpieza de emails erróneos (truncado después de .es/.com/.org)
  
- **Teléfono**:
  - Validación: 9 dígitos comenzando con '91'
  - Extracción del primer número válido
  - Información adicional almacenada en `info_adicional_contacto`
  - Relleno de nulos con `914800010`

**Columnas creadas**:
- `email_valido`, `email_estado`: Validación y clasificación de emails
- `telefono_valido`, `telefono_estado`: Validación y clasificación de teléfonos
- `info_adicional_contacto`: Información de teléfono adicional

**Resultado**: `data/museos_corregido.csv`

### 3. Analisis crimen.ipynb

**Objetivo**: Estandarizar datos de incidentes criminales.

**Transformaciones realizadas**:
- Eliminación de columnas innecesarias (Range, CallDateTime)
- Estandarización de City: normalización de valores a 'San Francisco'
- Formateo de OffenseDate a formato YYYY-MM-DD
- Reemplazo de AgencyId '1' por 'CA'

**Resultado**: `data/crimen_corregido.csv`

### 4. Recurso1.ipynb

Notebook de apoyo con ejemplos de perfilado de datos y operaciones básicas con pandas y SQLite.

## Dependencias

El proyecto usa un entorno virtual Python 3.12.10 con los siguientes paquetes principales:

```
pandas
numpy
ydata-profiling
openpyxl
ipywidgets
setuptools
```

## Instalación y Uso

### 1. Crear el entorno virtual (ya realizado)

```bash
python -m venv venv
.\venv\Scripts\Activate.ps1  # Windows PowerShell
```

### 2. Instalar dependencias

```bash
pip install pandas numpy ydata-profiling openpyxl ipywidgets
```

### 3. Ejecutar los notebooks

Abre los notebooks en Jupyter o VS Code:

```bash
jupyter notebook
```

O usa VS Code con la extensión Jupyter.

## Archivos Generados

### Reportes HTML (ProfileReport)
- `turismo.html`: Reporte de alojamientos originales
- `museos.html`: Reporte de museos originales
- `crimen.html`: Reporte de crimen original
- `museos_corregido.html`: Reporte de museos después de limpieza
- `crimen_corregido.html`: Reporte de crimen después de limpieza

### CSVs Corregidos
- `data/alojamientos_turisticos_corregido.csv`: Alojamientos limpios
- `data/museos_corregido.csv`: Museos validados y limpios
- `data/crimen_corregido.csv`: Crimen estandarizado

## Notas Técnicas

- **Encoding**: Archivos CSV usan `latin-1` y separador `;` para compatibilidad con datos españoles
- **Base de datos**: `my_database.db` es una base de datos SQLite para almacenamiento alternativo
- **Normalización Unicode**: Se usa `unicodedata.NFKD` para el manejo de tildes y caracteres especiales
- **Validación Regex**: Patrones específicos para email y teléfono según normativas españolas

## Autor

Proyecto de análisis de datos - Mis-recursos

## Fecha de Última Actualización

Diciembre 2, 2025
