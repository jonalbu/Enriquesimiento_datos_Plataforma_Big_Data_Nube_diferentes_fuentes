# EA3: Enriquecimiento de Datos en Plataforma de Big Data en la Nube

**Institución Universitaria Digital de Antioquia (IU Digital)**  
**Programa:** Ingeniería en Software / Tecnología en Desarrollo de Software  
**Asignatura:** Arquitectura Big Data (7° Semestre)  
**Actividad:** EA3. Enriquecimiento de Datos en Plataforma de Big Data en la Nube  
**Dataset Base:** FIFA 20 Complete Player Dataset (`players_20.csv` - 18,278 registros)
**Estudiante:** [Jonathan Alvarez Bustamante](https://github.com/jonalbu/Enriquesimiento_datos_Plataforma_Big_Data_Nube_diferentes_fuentes)  

---

## 1. Descripción de la Solución

Esta actividad implementa la tercera fase fundamental del ciclo de vida de Big Data: **Enriquecimiento e Integración de Datos Heterogéneos**. En esta etapa se combinan los datos base limpios con fuentes complementarias en **6 formatos distintos (JSON, CSV, XML, HTML, TXT, XLSX)** para potenciar las capacidades analíticas previas a la etapa de modelado (Actividad 4).

### Fuentes Heterogéneas Integradas (6 Formatos):
1. **JSON (`src/data/countries_info.json`):** Catálogo de 162 países con continente, confederación FIFA (UEFA, CONMEBOL, etc.), código ISO-3 y ranking tier (Cruce por `nationality`).
2. **CSV (`src/data/club_leagues.csv`):** Catálogo de 698 clubes con nombre de liga, país de liga, categoría y bandera de ligas Top 5 (Cruce por `club`).
3. **XML (`src/data/stadiums_info.xml`):** Información estructurada en XML con nombre de estadio y capacidad de aforo por club (Cruce por `club`).
4. **HTML (`src/data/national_trophies.html`):** Tabla web HTML con títulos de Copas del Mundo y trofeos continentales por país (Cruce por `nationality`).
5. **TXT (`src/data/player_contracts_status.txt`):** Archivo plano delimitado por pipe (`|`) con nivel de contrato y estrellas de reputación (Cruce por `sofifa_id`).
6. **XLSX (`src/data/sponsorship_tiers.xlsx`):** Hoja de cálculo Excel con información de patrocinadores principales y categoría comercial (Cruce por `club`).

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
    │   ├── countries_info.json       <- Fuente 1: JSON
    │   ├── club_leagues.csv          <- Fuente 2: CSV
    │   ├── stadiums_info.xml         <- Fuente 3: XML
    │   ├── national_trophies.html    <- Fuente 4: HTML
    │   ├── player_contracts_status.txt<- Fuente 5: TXT
    │   └── sponsorship_tiers.xlsx    <- Fuente 6: XLSX
    ├── enrichement.py                <- Script principal de enriquecimiento
    ├── enrichment.py                 <- Alias de compatibilidad
    ├── db/
    │   └── ingestion.db              <- Base de datos SQLite con tabla 'enriched_players'
    ├── xlsx/
    │   └── enriched_data.xlsx        <- Muestra representativa enriquecida en Excel
    └── static/
        └── auditoria/
            └── enriched_report.txt   <- Reporte de auditoria multiformato
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

## 5. Resumen del Reporte de Auditoría Multiformato

| Formato de Origen | Archivo | Clave de Enlace | Tasa de Cruce | Atributos Agregados |
| :---: | :--- | :---: | :---: | :--- |
| **JSON** | `countries_info.json` | `nationality` | **100.00%** | Continente, confederación FIFA, ISO-3, ranking tier |
| **CSV** | `club_leagues.csv` | `club` | **100.00%** | Liga, país de liga, división, flag Top 5 ligas |
| **XML** | `stadiums_info.xml` | `club` | **100.00%** | Nombre de estadio, capacidad de aforo |
| **HTML** | `national_trophies.html` | `nationality` | **100.00%** | Títulos de Copa del Mundo y trofeos continentales |
| **TXT** | `player_contracts_status.txt` | `sofifa_id` | **100.00%** | Nivel de contrato, estrellas de reputación |
| **XLSX** | `sponsorship_tiers.xlsx` | `club` | **100.00%** | Patrocinador principal, categoría de patrocinio |
| **Total Pipeline** | **6 Formatos** | **Relacional** | **100.00%** | **+20 nuevas columnas analíticas (44 total)** |
