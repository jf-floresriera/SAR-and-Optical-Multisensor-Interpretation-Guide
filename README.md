<p align="center">
  <a href="https://git.io/typing-svg">
    <img src="https://readme-typing-svg.demolab.com?font=Roboto+Mono&weight=600&size=23&pause=1000&color=1F4E79&center=true&vCenter=true&width=750&height=50&lines=Marco+Multisensor%3A+Sentinel-1+%C2%B7+ALOS-2+%C2%B7+Sentinel-2;Multisensor+Framework+%C2%B7+Marco+Multisensor;CEOS+v3.0+Layman's+SAR+Interpretation;Radiometric+Processing+%26+Lee+Adaptive+Filter" alt="Technical Header Animation / Animación Encabezado Técnico" />
  </a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-00529B?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.10+">
  <img src="https://img.shields.io/badge/Google%20Earth%20Engine-API-0F9D58?style=for-the-badge&logo=google-earth&logoColor=white" alt="GEE">
  <img src="https://img.shields.io/badge/Standard%20%2F%20Est%C3%A1ndar-CEOS%20v3.0-1F4E79?style=for-the-badge" alt="CEOS v3.0">
  <img src="https://img.shields.io/badge/License%20%2F%20Licencia-MIT-000000?style=for-the-badge" alt="License MIT">
</p>

<p align="center">
  <b>🇬🇧 English</b> · <b>🇪🇸 Español</b><br>
  <i>Bilingual document: each section is presented first in English, then in Spanish.<br>
  Documento bilingüe: cada sección se presenta primero en inglés y luego en español.</i>
</p>

---

## 1. System Description / Descripción del Sistema

### 🇬🇧 English

This repository hosts the automated workflow in both **Executable Script (`sar_guia_colab.py`)** and **Jupyter Notebook (`sar_guia_colab.ipynb`)** formats, designed for data acquisition, radiometric calibration, speckle filtering, and the generation of integrated multi-sensor products.

The methodology strictly replicates the specifications of the **CEOS Layman's SAR Interpretation Guide v3.0 (Rosenqvist et al., 2023)**, integrating C-band (Sentinel-1) and L-band (ALOS-2 PALSAR-2) microwave sensors with passive optical imagery (Sentinel-2 L2A).

### 🇪🇸 Español

Este repositorio alberga el flujo de trabajo automatizado en formato **Script Ejecutable (`sar_guia_colab.py`)** y **Jupyter Notebook (`sar_guia_colab.ipynb`)**, diseñado para la adquisición, calibración radiométrica, filtrado de speckle y generación de productos multisensor integrados.

La metodología replica de manera estricta las especificaciones del estándar **CEOS Layman's SAR Interpretation Guide v3.0 (Rosenqvist et al., 2023)**, integrando sensores de microondas en Banda C (Sentinel-1), Banda L (ALOS-2 PALSAR-2) y espectro óptico pasivo (Sentinel-2 L2A).

```
+---------------------------------------------------------------------------------+
|                    GENERAL WORKFLOW / FLUJO DE TRABAJO GENERAL                  |
+---------------------------------------------------------------------------------+
| 1. ACQUISITION           2. RADIOMETRIC PROCESSING        3. FINAL PRODUCTS     |
|    ADQUISICIÓN              PROCESAMIENTO RADIOMÉTRICO       PRODUCTOS FINALES  |
| +---------------------+  +-----------------------------+  +-------------------+ |
| | ALOS-2 (ScanSAR L2) |  | Linear Power Calibration    |  | 300 DPI PNG Panel | |
| | Sentinel-1 IW GRDH  |->| Lee Filter (ENL = 4.4)      |->| GeoTIFFs (UTM)    | |
| | Sentinel-2 L2A      |  | Gamma-Zero Normalization    |  | PDF Report        | |
| +---------------------+  | (Calibración / Filtro Lee / |  | manifiesto.json   | |
|                          |  Normalización Gamma-Zero)  |  +-------------------+ |
|                          +-----------------------------+                        |
+---------------------------------------------------------------------------------+
```

---

## 2. Requirements and Runtime Environment / Requisitos y Entorno de Ejecución

### 🇬🇧 English

The code is optimized for cloud computing environments (Google Colab) or local geospatial processing servers.

#### Main Dependencies
* **`earthengine-api` & `geemap`:** Interface and querying of raster collections in Google Earth Engine.
* **`rasterio` & `pyproj`:** Reprojection, geospatial array handling, and GeoTIFF export.
* **`reportlab`:** Vector generation of the scientific report in PDF format.
* **`asf_search`:** Optional query and integration of SLC products and RTC derivatives from the Alaska Satellite Facility / NASA JPL.

### 🇪🇸 Español

El código está optimizado para entornos de computación en la nube (Google Colab) o servidores locales de procesamiento geoespacial.

#### Dependencias Principales
* **`earthengine-api` & `geemap`:** Interfaz e interrogación de colecciones ráster en Google Earth Engine.
* **`rasterio` & `pyproj`:** Reproyección, manejo de matrices geoespaciales y exportación GeoTIFF.
* **`reportlab`:** Generación vectorial del informe científico en formato PDF.
* **`asf_search`:** Consulta e integración opcional de productos SLC y derivados RTC del Alaska Satellite Facility / NASA JPL.

### Installation Command / Comando de Instalación
```bash
pip install -q rasterio reportlab asf_search geemap pyproj earthengine-api
```

---

## 3. Configuration Guide: Parameters to Modify / Guía de Configuración: Parámetros a Modificar

### 🇬🇧 English

To adapt the workflow to a specific study area, **only the initial configuration block (Section 1 of the code) needs to be edited**.

### 🇪🇸 Español

Para adaptar el flujo a un área de estudio específica, **únicamente debe editarse el bloque de configuración inicial (Sección 1 del código)**.

<details open>
<summary><b>Table of Modifiable Parameters / Tabla de Parámetros Modificables</b></summary>

<br>

| Variable | Type / Tipo | Default / Valor por Defecto | Description (EN) | Descripción (ES) |
| :--- | :--- | :--- | :--- | :--- |
| `GEE_PROJECT` | `str` | `"wide-origin-466923-d8"` | Identifier of the project enabled in Google Cloud. | Identificador del proyecto habilitado en Google Cloud. |
| `AOI_BBOX` | `tuple` | `(-8163798.9, 419908.3, ...)` | Bounding box of the study area (XY coordinates). | Bounding Box de la zona de estudio (Coordenadas XY). |
| `AOI_CRS` | `str` | `"EPSG:3857"` | Spatial reference system of the input bounding box. | Sistema de Referencia Espacial del BBOX de entrada. |
| `CRS_TRABAJO` | `str` | `"EPSG:32618"` | Output UTM projection (e.g., UTM 18N). | Proyección UTM de salida (Ej. UTM 18N). |
| `FECHA_INICIO` | `str` | `"2023-01-01"` | Lower bound of the temporal search window. | Límite inferior de la ventana temporal de búsqueda. |
| `FECHA_FIN` | `str` | `"2025-12-31"` | Upper bound of the temporal search window. | Límite superior de la ventana temporal de búsqueda. |
| `VENTANA_DIAS` | `int` | `15` | Maximum tolerated temporal offset between sensors. | Tolerancia máxima de desfase temporal entre sensores. |
| `S2_NUBES_AOI_MAX` | `float` | `10.0` | Maximum tolerated cloud cover threshold within the AOI (%). | Umbral máximo tolerado de cobertura nubosa en el AOI (%). |
| `FECHAS_MANUALES` | `dict` | `None` | Force exact dates `{"alos": "YYYY-MM-DD", ...}`. | Forzar fechas exactas `{"alos": "YYYY-MM-DD", ...}`. |
| `GUARDAR_EN_DRIVE` | `bool` | `True` | Mount and save directly to Google Drive. | Montar y guardar directamente en Google Drive. |

</details>

---

## 4. Technical Processing Architecture / Arquitectura del Procesamiento Técnico

### 🇬🇧 English

The algorithm operates in three strict phases, recording every variable in a persistent control structure (`manifiesto.json`).

### 🇪🇸 Español

El algoritmo opera en tres fases estrictas, registrando cada variable en una estructura de control persistente (`manifiesto.json`).

<details>
<summary><b>Phase 1: Acquisition and Temporal Alignment / Fase 1: Adquisición y Alineación Temporal</b></summary>

#### 🇬🇧 English

1. **Anchor Sensor Selection:** The system evaluates the available ALOS-2 ScanSAR scenes (`JAXA/ALOS/PALSAR-2/Level2_2/ScanSAR`) because of their lower revisit frequency.
2. **Multispectral Matching:** It locates Sentinel-1 IW GRDH (`COPERNICUS/S1_GRD_FLOAT`) and Sentinel-2 L2A (`COPERNICUS/S2_SR_HARMONIZED`) acquisitions that minimize the total time interval ($\Delta t \le \text{VENTANA\_DIAS}$).
3. **Cloud Cover Mapping:** It computes the cloud fraction within the exact AOI by analyzing the Sentinel-2 SCL (Scene Classification Layer) band.

#### 🇪🇸 Español

1. **Selección del Sensor Ancla:** El sistema evalúa las escenas disponibles de ALOS-2 ScanSAR (`JAXA/ALOS/PALSAR-2/Level2_2/ScanSAR`) debido a su menor frecuencia de revisita.
2. **Coincidencia Multiespectral:** Localiza capturas de Sentinel-1 IW GRDH (`COPERNICUS/S1_GRD_FLOAT`) y Sentinel-2 L2A (`COPERNICUS/S2_SR_HARMONIZED`) que minimicen el intervalo temporal total ($\Delta t \le \text{VENTANA\_DIAS}$).
3. **Mapeo de Cobertura Nubosa:** Calcula la fracción de nubes en el AOI exacto analizando la banda SCL (Scene Classification Layer) de Sentinel-2.
</details>

<details>
<summary><b>Phase 2: Applied Physics and Mathematics / Fase 2: Física y Matemática Aplicada</b></summary>

#### 🇬🇧 English

* **ALOS-2 Radiometric Conversion:**
  $$\gamma^0 = 10 \cdot \log_{10}(\text{DN}^2) - 83.0 \quad [\text{dB}]$$
* **Sentinel-1 Normalization:**
  $$\gamma^0 = \frac{\sigma^0}{\cos \theta_{\text{inc}}}$$
* **Adaptive Speckle Filtering (Lee Filter):**
  Executed on linear power before conversion to decibels ($\text{dB}$):
  $$\hat{R} = \bar{I} + k(I - \bar{I}), \quad k = \max\left(0, 1 - \frac{C_u^2}{C_i^2}\right)$$
* **Linear Differences and Polarimetric Ratios:**
  Direct computation of differences in linear power ($VV - VH$ and $HH - HV$) and ratios on a logarithmic scale ($VV/VH$ in $\text{dB}$) to differentiate volume scattering mechanisms from surface scattering.
* **OPERA S1 RTC Integration:**
  Independent module that downloads and integrates the collection with advanced NASA-JPL terrain correction (`OPERA/RTC/L2_V1/S1`).

#### 🇪🇸 Español

* **Conversión Radiométrica ALOS-2:**
  $$\gamma^0 = 10 \cdot \log_{10}(\text{DN}^2) - 83.0 \quad [\text{dB}]$$
* **Normalización Sentinel-1:**
  $$\gamma^0 = \frac{\sigma^0}{\cos \theta_{\text{inc}}}$$
* **Filtrado Adaptativo de Speckle (Filtro de Lee):**
  Ejecutado sobre potencia lineal antes de la conversión a decibelios ($\text{dB}$):
  $$\hat{R} = \bar{I} + k(I - \bar{I}), \quad k = \max\left(0, 1 - \frac{C_u^2}{C_i^2}\right)$$
* **Diferencias Lineales y Cocientes Polarimétricos:**
  Cálculo directo de diferencias en potencia lineal ($VV - VH$ y $HH - HV$) y ratios en escala logarítmica ($VV/VH$ en $\text{dB}$) para diferenciar mecanismos de dispersión de volumen frente a dispersión de superficie.
* **Integración OPERA S1 RTC:**
  Módulo independiente que descarga e integra la colección con corrección topográfica avanzada del NASA-JPL (`OPERA/RTC/L2_V1/S1`).
</details>

<details>
<summary><b>Phase 3: Deliverable Products / Fase 3: Productos Entregables</b></summary>

#### 🇬🇧 English

* **3x4 Interpretation Panel (300 DPI):** Comparative matrix with grayscale stretched by percentile histogram (2–98%) and polarimetric RGB composites.
* **Processed GeoTIFFs:** Co-registered to a common 10-meter grid.
* **PDF Scientific Report:** Automatically generated document with zonal backscatter statistics and a correlation table with land-cover signatures from the CEOS v3.0 guide.

#### 🇪🇸 Español

* **Panel de Interpretación 3x4 (300 DPI):** Matriz comparativa con escala de grises ajustada por histograma percentil (2–98%) y composiciones RGB polarimétricas.
* **GeoTIFFs Procesados:** Co-registrados a la grilla común de 10 metros.
* **Reporte Científico PDF:** Documento autogenerado con estadísticas de retrodispersión zonal y tabla de correlación con firmas de cobertura de la guía CEOS v3.0.
</details>

---

## 5. Sensor Comparison Table / Cuadro Comparativo de Sensores

| Sensor | Band / Banda | Frequency / Frecuencia | Wavelength / Longitud de Onda ($\lambda$) | Polarizations / Polarizaciones | Interaction Mechanism (EN) | Mecanismo de Interacción (ES) |
| :--- | :---: | :---: | :---: | :---: | :--- | :--- |
| **Sentinel-1** | C | 5.405 GHz | $\approx 5.55\text{ cm}$ | VV, VH | Upper canopy, low vegetation, surface roughness. | Dosel superior, vegetación baja, rugosidad superficial. |
| **ALOS-2 PALSAR-2** | L | 1.2365 GHz | $\approx 24.24\text{ cm}$ | HH, HV | Canopy penetration, interaction with main branches, trunks, and biomass. | Penetración de dosel, interacción con ramas principales, troncos y biomasa. |
| **Sentinel-2** | Optical / Óptico | — | $492 \text{ nm} - 833\text{ nm}$ | B2, B3, B4, B8 | Solar reflectance of the vegetation cover and NDVI. | Reflectancia solar de la cubierta vegetal y NDVI. |

---

## 6. Scientific References / Referencias Científicas

1. **Rosenqvist, A., Killough, B., Dyke, G., & Borges, D. (2023).** *A Layman's Interpretation Guide to L-band and C-band SAR data (v3.0)*. Committee on Earth Observation Satellites (CEOS).
2. **Lee, J.-S. (1980).** *Digital image enhancement and noise filtering by use of local statistics*. IEEE Transactions on Pattern Analysis and Machine Intelligence, 2(2), 165-168.
3. **Shimada, M. (2010).** *Ortho-rectification and slope correction of SAR data using DEM*. IEEE JSTARS, 3(4), 657-671.
4. **Cloude, S. R. (2007).** *The dual polarization entropy/alpha decomposition*. Proceedings of PolInSAR, ESA.
5. **Gorelick, N. et al. (2017).** *Google Earth Engine: Planetary-scale geospatial analysis for everyone*. Remote Sensing of Environment, 202, 18-27.
