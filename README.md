**Challenge: Asset Change Detection and Risk Assessment using Satellite Imagery**
## **Introduction**
Critical infrastructure such as pipelines, railways, and roads are often exposed to environmental risks, including vegetation encroachment, nearby construction, and natural changes like river movements. This report presents a methodology for monitoring such assets using satellite imagery to detect environmental changes and assess the risk posed to specific segments of the asset.
## **Methodology**
### **1. Coordinate System Transformation**
To accurately monitor environmental changes, it was essential to align the spatial data of the asset with the satellite imagery. The asset data, provided in GeoJSON format, was reprojected using the **"Reproject Layer"** tool in QGIS to match the coordinate reference system (CRS) of the satellite images. This transformation ensured accurate spatial analysis and consistency between the asset and the imagery. (**Reproject\_Asset\_line.geojson**)
### **2. Segmenting the Asset**
The asset was segmented into smaller parts to analyze risks in detail. Using QGIS’s **"Split Lines by Maximum Length"** tool, the asset was divided into uniform 25-meter segments. 
### **3. Detecting Changes Around the Asset**
#### *1. Buffer Creation*
To focus on environmental changes close to the asset, a 50-meter buffer zone was created using the **"Buffer"** tool in QGIS around asset line. This buffer represents the critical zone where changes, such as vegetation growth or nearby construction, could impact the asset’s safety. (**50m\_Buff\_Asset\_line.geojson**)
#### *2. Change Detection*
Two satellite images from different time periods were compared to identify environmental changes within the buffer zone. By enhancing the contrast and using the 4-3-2 band combination, significant changes such as **tree growth, river expansion,** and **road network** were highlighted. This visual analysis allowed for clear detection of differences between the two images.
Created a shapefile and marked all the polygons manually by toggling the raster images. In the attribute table, the **Class** represents the detected environmental changes category. (**Change\_Area.shp**)
###
### **4. Risk Classification**
#### *1. Joining Change Data with Asset Segments*
The next step was to connect the change area shapefile with the 25-meter asset segments file. This was done using the **"Join Attributes by Nearest"** tool in QGIS, which linked change areas to the nearest asset segments within a 10-meter distance. This process helped identify segments that could be affected by nearby changes, such as encroaching trees or construction sites. (This can also be done by using buffer and clip tools)

A new attribute named **"Risk Level"** was added to the asset layer. Segments within 10 meters of detected changes were classified as **High Risk**, as they were more likely to be affected by the environmental changes. Segments located farther from the changes were classified as **Low Risk**, indicating a reduced likelihood of impact. This file also has the **distance (m)** and **class** name attributes. (**Asset\_line\_Risk\_Status.shp**)
### **5. Visualizing Raster and Change Data**
To visualize the analysis results, including the satellite images and detected change areas around the asset, various Python libraries were used including **numpy**, **geopandas**, **rasterio** and **matplotlib** were used to load and manipulate geospatial data. Change area shape file along with satellite images are plotted. (only 5 are shown due to rendering performance. (**01-plot-shape-file.ipynb)**

By using **rioxarray** and **earthpy** the complete Change area geojson file is plotted along with satellite images (**02-plot-geojson-file.ipynb**)

To run the code add raster images in **Dataset** folder.

**Risk_Assessment.qgz** is also attached to visualizelayers in QGIS.