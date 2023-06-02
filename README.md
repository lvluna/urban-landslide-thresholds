[![DOI]()

# urban-landslide-thresholds
*Bayesian quantile regression to learn rainfall intensity duration thresholds for shallow landsliding in urban areas globally*

This repository contains notebooks that perform the analysis reported by:

**Luna, L.V., Arango-Carmona, M.I., Veh, G., Lewis, E., Ozturk, U., Korup, O. Urban landslides triggered under similar rainfall conditions in cities globally. Submitted to *Nature*.**

### 01_CompileLandslideInventories.ipynb
This Python Jupyter notebook:
- Reads original landslide inventory data from 10 publicly available landslide inventories
- Converts all inventories to a common format, assigning a trigger, type, material, and spatial uncertainty where possible
- Binds formatted inventories into a global compilation
- Identifies which landslides occurred in urban areas according to the Global Human Settlement Layer Urban Centre Database (GHS-UCDB)
- Removes duplicates between inventories
- Subsets global compilation to landslides that have a documented rainfall trigger, date, and occurred in urban areas for further analysis

**Original data used:**

*see code for citations, access, version, and licensing information*

**Landslide inventories**
- [NASA Global Landslide Catalog](https://doi.org/10.1016/j.geomorph.2015.03.016)
- [Global Fatal Landslide Database V2](https://doi.org/10.5194/nhess-18-2161-2018)
- [Geoscience Australia Landslide Search](http://pid.geoscience.gov.au/dataset/ga/74273)
- [Landslide and Torrential Colombia Database](https://geohazards.com.co/ )
- [FraneItalia V3](https://doi.org/10.17632/zygb8jygrw.3)
- [GNS New Zealand Landslide Database](https://doi.org/10.1007/s10346-017-0843-6)
- [Landslide Inventory Rwanda](https://doi.org/10.4121/15040446.v1)
- [Kentucky Geological Survey Landslide Inventory](https://doi.org/10.13023/kgs.ic31.12)
- [Digital Data Series DGS06-3 Landslides in New Jersey](https://www.state.nj.us/dep/njgs/geodata/dgs06-3.htm)
- [Seattle Historic Landslide Locations ECA](https://data-seattlecitygis.opendata.arcgis.com/datasets/SeattleCityGIS::historic-landslide-locations-eca/about)

**Urban areas**
- [Global Human Settlement Layer Urban Centre Database](https://jeodpp.jrc.ec.europa.eu/ftp/jrc-opendata/GHSL/GHS_STAT_UCDB2015MT_GLOBE_R2019A/V1-2/)


### 02_IdentifyGauges.ipynb
This Python Jupyter notebook:
- Identifies all gauges in the [Global Sub-Daily Rainfall Dataset (GSDR)](https://journals.ametsoc.org/view/journals/clim/32/15/jcli-d-18-0143.1.xml) (Lewis, 2018) within 25 km of each landslide and records their distance to the landslide.
- Identifies all cities from the GHS-UCDB with at least 5 landslides that have nominal rainfall data coverage for further analysis

### 03_ExtractGSDR.ipynb
This Python Jupyter notebook:
- Extracts landslide triggering event rainfall for each landslide from each nearby gauge from the GSDR
- Extracts annual block maxima at a range of durations for each year on record from each nearby gauge

### 04_Extract_SA_CO.ipynb
This Python Jupyter notebook extracts metrics for landslides in Durban and Medellin. It's similar to 03_ExtractGSDR, but processes precipitation data for Durban from the South African Weather Service and for Medellin from the Colombian [Instituto de Hidrología, Meteorología y Estudios Ambientales (IDEAM)](http://dhime.ideam.gov.co/atencionciudadano/).


### 05_CombinePrep.ipynb
This Python Jupyter notebook combines landslide triggering rainfall and annual
block maxima from all datasets, selects the appropriate gauge for each landslide,
and subsets to cities with at least 5 landslide points that meet all selection
criteria. After these steps, the landslide triggering rainfall data is ready for quantile regression.

This notebook produces `lsdata_rain.csv`, which contains event rainfall metrics
for each landslide included in the quantile regression analysis.

### 06_BayesianQuantileRegression.Rmd
This R Markdown notebook uses Bayesian multi-level quantile regression to estimate 10th percentile, 50th percentile, and 90th percentile rainfall intensity-duration thresholds for 26 cities and global mean thresholds across all cities.
