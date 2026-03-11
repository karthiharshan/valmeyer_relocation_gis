# valmeyer_relocation_gis
Site suitability and emergency management mapping for Valmeyer, Illinois, using ArcGIS to update parcels, calculate terrain and distance attributes, and share results via a public web map.

## Project objectives

- Update and maintain parcel GIS layers for old and new Valmeyer so they can be used reliably for assessment, planning, and emergency management.
- Derive parcel-level elevation, slope, and distance metrics to support analysis of flood risk, relocation effort, and future resilience.
- Create a new, connected roads layer suitable for emergency vehicle routing and future network analysis.
- Publish a browser-based web map via ArcGIS Online to communicate results in an accessible way. 

---

## Data

All source data were provided as a zipped file geodatabase, containing: 

- **Old Valmeyer parcels**  
  - Polygon feature class for the original town location  
  - Includes existing elevation and slope attributes

- **New Valmeyer parcels (incomplete)**  
  - Polygon feature class for the relocated town  
  - Requires elevation, slope, and distance attributes

- **Digital Elevation Model (DEM)**  
  - Raster DEM covering the study area  
  - Used to derive elevation, slope, and distance rasters

Additional derived datasets (created in this project): 

- Slope raster (degrees), derived from the DEM  
- Euclidean distance raster from the old town location  
- Digitised road lines for the new town

---

## Methods

### 1. Derive terrain and distance rasters

1. Create a **slope raster** (in degrees) from the DEM.  
2. Generate an **Euclidean distance raster** from the old town location, with:
   - Maximum distance: 5000 m  
   - Output cell size: 10 m  
   - “Snap Raster” and “Extent” set to the DEM to ensure alignment. 

### 2. Attach parcel-level statistics

Use Zonal Statistics (or Zonal Statistics as Table) with the new Valmeyer parcels as zones and the rasters as inputs: 

- From the **DEM**, calculate:
  - `min_elev` – minimum elevation per parcel  
  - `max_elev` – maximum elevation per parcel  
  - `mean_elev` – mean elevation per parcel  

- From the **slope raster**, calculate:
  - `min_slope` – minimum slope (degrees) per parcel  
  - `max_slope` – maximum slope (degrees) per parcel  
  - `mean_slope` – mean slope (degrees) per parcel  

- From the **distance raster**, calculate:
  - `move_dist` – mean distance (meters) from the old town to each new parcel  

After generating tables, join them back to the new parcels feature class and rename fields directly (not via aliases) so they display correctly in ArcGIS Online. 

### 3. Digitise new roads

- Create a new line feature class for **roads in new Valmeyer**.  
- Digitise lines in the gaps between parcels, approximating the road network and ensuring connectivity for future network analyses and emergency routing.  
- Use snapping settings that make it easier to connect road segments without accidentally snapping to parcel boundaries.

### 4. Prepare layers for web mapping

- Export the following as shapefiles for upload to ArcGIS Online:  
  - Old Valmeyer parcels  
  - New Valmeyer parcels (with elevation, slope, and distance attributes)  
  - New Valmeyer roads  

- Zip the shapefiles and upload them as items to ArcGIS Online. 

---

## Web map

The final **ArcGIS Online web map** contains: 

1. **Old parcels layer** – symbolised with a single colour to show the original town location.  
2. **New parcels layer** – symbolised by `mean_elev` to illustrate elevation patterns and relative flood safety.  
3. **New roads layer** – digitised road lines supporting visualisation of access and connectivity.

This web map allows emergency management staff, planners, and the public to: 

- Compare old vs. new town locations.  
- Explore elevation and slope characteristics parcel by parcel.  
- Understand the approximate relocation distance and new road network layout.

Web map: (https://ucd-cpe.maps.arcgis.com/home/item.html?id=d15e3d1021e44374b2236021df4fc3bd)
