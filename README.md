# EA3: Enriquecimiento de Datos en Plataforma de Big Data en la Nube

**Institución Universitaria Digital de Antioquia (IU Digital)**  
**Programa:** Ingeniería en Software / Tecnología en Desarrollo de Software  
**Asignatura:** Arquitectura Big Data (7° Semestre)  
**Actividad:** EA3. Enriquecimiento de Datos en Plataforma de Big Data en la Nube  
**Dataset Base:** FIFA 20 Complete Player Dataset (`players_20.csv` - 18,278 registros)  

---

## 1. Descripción de la Solución

Esta actividad implementa la tercera fase fundamental del ciclo de vida de Big Data: **Enriquecimiento e Integración de Datos Heterogéneos**. En esta etapa se combinan los datos base limpios con fuentes complementarias en múltiples formatos para potenciar las capacidades analíticas previas al modelado (Actividad 4).

### Flujo del Pipeline de Enriquecimiento:
1. **Carga del Dataset Base:** Extracción de 18,278 registros de jugadores y preparación del esquema analítico base.
2. **Lectura de Fuentes Adicionales Multiformato:**
   - 📄 **JSON (`src/data/countries_info.json`):** Catálogo de 162 países con continente, confederación FIFA (UEFA, CONMEBOL, etc.), código ISO-3 y nivel de ranking.
   - 📊 **CSV (`src/data/club_leagues.csv`):** Catálogo de 698 clubes con nombre de liga, país de la liga, categoría y bandera de pertenencia a las Top 5 ligas europeas.
3. **Cruce e Integración de Información (Joins):**
   - Integración mediante `LEFT JOIN` (merge) utilizando como claves de enlace `nationality` y `club`.
   - Garantía de **cero pérdida de registros** (18,278 registros preservados al 100%).
4. **Ingeniería de Características Enriquecidas:**
   - **`wage_tier`:** Clasificación salarial en 4 estratos (*Base*, *Medio*, *Alto*, *Elite*).
   - **`market_value_tier`:** Segmentación por valor de mercado (*Desarrollo*, *Profesional*, *Estelar*, *Superstar*).
   - **`is_intercontinental`:** Identificación de talentos no europeos compitiendo en ligas top de Europa.
   - **`star_rating_index`:** Índice compuesto ponderado de rendimiento deportivo (70%) y valor comercial (30%).
5. **Almacenamiento y Generación de Evidencias:**
   - Almacenamiento en SQLite (`src/db/ingestion.db`) en la tabla `enriched_players`.
   - Generación de muestra representativa en Excel [`src/xlsx/enriched_data.xlsx`](src/xlsx/enriched_data.xlsx).
   - Generación de reporte de auditoría [`src/static/auditoria/enriched_report.txt`](src/static/auditoria/enriched_report.txt).
6. **Automatización con GitHub Actions:** Pipeline en [`.github/workflows/bigdata.yml`](.github/workflows/bigdata.yml) que ejecuta el flujo completo y publica los artefactos.

---

## 2. Estructura del Proyecto

```text
Tarea_3/
├── players_20.csv                    <- Dataset base (FIFA 20 Players)
├── setup.py                          <- Empaquetado del proyecto
├── requirements.txt                  <- Dependencias del entorno
├── README.md                         <- Documentacion y trazabilidad
├── .gitignore                        <- Exclusiones de Git
├── .github/
│   └── workflows/
│       └── bigdata.yml               <- Pipeline de GitHub Actions
└── src/
    ├── data/
    │   ├── countries_info.json       <- Fuente complementaria en JSON
    │   └── club_leagues.csv          <- Fuente complementaria en CSV
    ├── enrichement.py                <- Script principal de enriquecimiento
    ├── enrichment.py                 <- Alias de compatibilidad
    ├── db/
    │   └── ingestion.db              <- Base de datos SQLite con tabla 'enriched_players'
    ├── xlsx/
    │   └── enriched_data.xlsx        <- Muestra representativa enriquecida en Excel
    └── static/
        └── auditoria/
            └── enriched_report.txt   <- Reporte de auditoria de integracion
```

---

## 3. Instrucciones de Ejecución Local

### Prerrequisitos
- Python 3.9 o superior.
- Git.

### Paso 1: Clonar el repositorio
```bash
git clone https://github.com/TU_USUARIO/TU_REPOSITORIO.git
cd TU_REPOSITORIO
```

### Paso 2: Crear y activar entorno virtual
- **En Windows:**
  ```bash
  python -m venv venv
  .\venv\Scripts\activate
  ```
- **En Linux / macOS:**
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```

### Paso 3: Instalar dependencias
```bash
pip install -r requirements.txt
```

### Paso 4: Ejecutar el pipeline de enriquecimiento
```bash
python src/enrichement.py
```

---

## 4. Automatización con GitHub Actions

El archivo [`.github/workflows/bigdata.yml`](.github/workflows/bigdata.yml) se ejecuta ante cada `push` o ejecución manual:
1. Configura el entorno virtual con **Python 3.11**.
2. Instala las dependencias del proyecto.
3. Ejecuta `python src/enrichement.py`.
4. Valida la existencia física de la base de datos, el archivo Excel y el informe de auditoría.
5. Imprime el reporte de auditoría en la consola de GitHub Actions.
6. Publica y almacena los artefactos (`enrichment-evidences`) para su descarga y evaluación.

---

## 5. Resumen del Reporte de Auditoría (Resultados de Integración)

| Dimensión / Métrica | Dataset Base | Dataset Enriquecido | Resultado del Cruce |
| :--- | :---: | :---: | :---: |
| **Total Registros** | 18,278 | 18,278 | **100% registros preservados** (0 pérdidas) |
| **Total Columnas** | 24 | 36 | **+12 nuevas columnas analíticas** |
| **Cruce Países (JSON)** | - | 18,278 | **100.00% match rate** (`nationality`) |
| **Cruce Clubes (CSV)** | - | 18,278 | **100.00% match rate** (`club`) |
| **Talentos Intercontinentales** | - | 147 | Identificados exitosamente |
| **Jugadores en Top 5 Ligas** | - | 596 | Identificados exitosamente |
| **Estado de Integridad** | Base | Enriquecido | **EXITOSO - Listo para Modelado (EA4)** |
