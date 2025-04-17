# Rural Healthcare Accessibility Analysis – Wardha West, Maharashtra

## Project Overview
This document outlines the complete workflow for assessing rural healthcare accessibility in Wardha West, Maharashtra using QGIS and geospatial datasets sourced from Google Earth Engine and other public repositories. It details all raster and vector layers used, their significance, preprocessing steps, and the spatial queries executed.

---

## Data Layers

### Raster Layers
- **Vegetation (NDVI)**  
  - Source: Google Earth Engine  
  - Use: Indicates green cover; low vegetation zones favor infrastructure development.  
- **Land Cover / Land Use**  
  - Source: ESA WorldCover via GEE  
  - Use: Differentiates built-up, agricultural, and natural areas.  
- **Population Density**  
  - Source: WorldPop / Census data  
  - Use: Identifies high-demand zones for healthcare services.  
- **Elevation (DEM)**  
  - Source: SRTM DEM via GEE  
  - Use: Analyses terrain barriers affecting accessibility.  
- **Temperature (Variability)**  
  - Source: MODIS / ERA5  
  - Use: Highlights areas with extreme temperatures that may hinder access.  
- **Rainfall Patterns**  
  - Source: CHIRPS / IMD data  
  - Use: Accounts for seasonal flooding risk.  
- **Air Quality (PM2.5, PM10)**  
  - Source: Sentinel-5P  
  - Use: Identifies environmental health risk zones.  

### Vector Layers
- **Road Networks**  
  - Source: OpenStreetMap / GEE  
  - Use: Primary connectivity for healthcare access.  
- **Rivers & Water Bodies**  
  - Source: GEE / Local GIS portals  
  - Use: Natural barriers and waterborne disease risk zones.  
- **Forests**  
  - Source: ESA WorldCover  
  - Use: Impacts accessibility; exclusion zones for construction.  
- **Administrative Boundary**  
  - Source: GADM / Bhuvan  
  - Use: Clipping analysis to study area.  
- **Hospitals & PHCs & Nursing Schools & Pharmacies**  
  - Source: MoHFW / Geocoded local data  
  - Use: Existing healthcare infrastructure points.  
- **Residential Complexes / Settlements**  
  - Source: GEE / Census shapefiles  
  - Use: Population centers requiring service.  
- **Health Facilities (General)**  
  - Source: Government health department  
  - Use: Combined point layer for service coverage.  
- **Public Transport Routes**  
  - Source: State Transport / OSM  
  - Use: Alternate connectivity analysis.  
- **Electricity Grid**  
  - Source: State Electricity Boards  
  - Use: Infrastructure support analysis.  
- **Seismic Hazard Zones**  
  - Source: GSI / Bhuvan  
  - Use: Risk assessment for facility locations.  
- **Disaster Response Centers**  
  - Source: NDMA / Local reports  
  - Use: Emergency planning support.  
- **Water Quality Stations**  
  - Source: CGWB / Survey data  
  - Use: Drinking water risk identification.  
- **Disease Incidence Data (Waterborne & Viral)**  
  - Source: NFHS / State health reports  
  - Use: Risk zone mapping.  
- **Poverty & Socio-economic Index**  
  - Source: SECC / NFHS  
  - Use: Prioritizing vulnerable populations.  
- **Ambulance Network**  
  - Source: Local health authorities  
  - Use: Emergency response analysis.  
- **Education Levels (Indicator)**  
  - Source: Census of India  
  - Use: Correlation with health awareness.  

---

## Spatial Queries & Analysis

1. **Underserved Residential Areas**  
   - **Expression:**  
     ```
     (RES_BUFFER - (HEALTHCARE_BUFFER ∪ ROAD_BUFFER)) ∩ GREEN_BUFFER
     ```  
   - **Purpose:** Identifies residential zones not served by healthcare or roads but adjacent to green spaces.

2. **Road Network Gaps**  
   - **Expression:**  
     ```
     (PHC ∪ HOSP ∪ NURSING ∪ PHARMA) - BUFFER(ROADS)
     ```  
   - **Purpose:** Highlights healthcare facilities lacking direct road connectivity.

3. **Construction-Ready Housing Zones**  
   - **Expression:**  
     ```
     CLIP((PHC ∪ HOSP ∪ NURSING ∪ PHARMA) - BUFFER(RESIDENTIAL), BOUNDARY)
     ```  
   - **Purpose:** Locates service points outside existing residential influence.

4. **Waterborne Disease Risk Zones**  
   - **Expression:**  
     ```
     SYM_DIF(WATERBORNE_CASES - BUFFER(HEALTHCARE ∪ RESIDENTIAL))
     ```  
   - **Purpose:** Maps high-risk waterborne disease areas lacking healthcare access.

5. **Viral Spread in High Population Areas**  
   - **Expression:**  
     ```
     INTERSECTION(REPORTED_VIRAL_CASES, BUFFER(HEALTHCARE ∪ RESIDENTIAL))
     ```  
   - **Purpose:** Shows viral cases near population centers and healthcare.

6. **Optimal PHC Placement**  
   - **Expression:**  
     ```
     BUFFER(RESIDENTIAL - ROAD)
     ```  
   - **Purpose:** Suggests new PHC sites in populated, unserviced areas.

7. **Priority Zone – Healthcare Coverage Exclusion**  
   - **Steps:**  
     - Buffer hospitals & PHCs (2 km)  
     - Subtract from study boundary to get uncovered areas.

8. **Priority Zone – Residential Influence**  
   - **Steps:**  
     - Buffer residential complexes (2 km) to identify demand areas.

9. **Priority Zone – Exclusion Zones**  
   - **Steps:**  
     - Buffer water bodies & forests (1 km) as exclusion zones.

10. **Buildable Area Extraction**  
    - **Expression:**  
      ```
      Boundary - (Water_Buffer ∪ Forest_Buffer)
      ```  
    - **Purpose:** Derives potential buildable sites.

11. **Existing Service Coverage Removal**  
    - **Expression:**  
      ```
      Boundary - (Hospital_Buffer ∪ PHC_Buffer)
      ```  
    - **Purpose:** Isolates areas lacking current service coverage.

12. **Hotspot Identification**  
    - **Expression:**  
      ```
      Buildable_Area ∩ Residential_Complexes
      ```  
    - **Purpose:** Pinpoints high-priority hotspots for new healthcare facilities.

---

## Workflow Steps

1. **Acquire Data** via Google Earth Engine and public repositories.
2. **Preprocess Layers**: CRS alignment, clipping, normalization.
3. **Create Buffers & Perform Spatial Queries** in QGIS.
4. **Generate Thematic Maps** showing suitability, risk, and coverage.
5. **Derive Insights** for infrastructure planning and policy-making.
6. **Validate** with field surveys and stakeholder consultations.

---

## References
- Abulibdeh, A., Alshammari, H., Al-Hajri, N. et al. (2025). Geospatial Assessment of Healthcare Accessibility and Equity in Qatar. *J Geovis Spat Anal*, 9(2).  
- Various public datasets: Google Earth Engine, OpenStreetMap, Bhuvan, WorldPop, NFHS, IMD, ESA WorldCover.

---

*README.md generated programmatically.*
