# Observed and Predicted Water Use over the Jordan River Basin
Calista Moore | CSU WR514 Final Project | May 1, 2022

# Introduction
This page describes the use of WaPOR data from the Jordan River Basin from over a decade to analyze historic changes and predict future trends in land use, precipitation, and evapotranspiration and interception.

## Defining the Issue
Jordan is one of the most water-scarce countries in the world, with major aqueducts drying up and the government in search of alternative water sources to sustain its growing population (FAO and IHE Delft, 2020). Jordan houses large refugee communities and its economy relies on tourism and agricultural exports, including water-intensive crops of rice and almonds as well as dairy and poultry farms, speeding up the depletion of its water resources (U.S. Department of Commerce, n.d.). Poor communities face days without water supply and private water companies have no incentive to close the gap due to the growing cost of water (United States Government, 2018). Alternative sources of water have been identified, including major desalination and reuse projects, but demand continues to rise and outpace new water sources. The Jordan River Basin is a major source of water for the country. 

<b><i>Jordan's worsening water shortage will have growing impacts on major sectors of the economy in addition to the wellbeing of its citizens and refugees.</b></i>

This project will assess how land use, precipitation, and evapotranspiration and interception have changed in Jordan from 2009-2020 and predict how Jordan’s water resources will change in the future based on trends from available data, especially for farmers and cities.

## Research Questions and Objectives
The following research questions are posed:
1. How have land use, precipitation, and evapotranspiration and interception changed from 2009-2020 over the Jordan River Basin?
2. How might these historic trends predict future changes, especially for cropland?

# Methods

## Description of Datasets

### Three datasets are downloaded from the FAO WaPOR catalog (FAO, 2020) for the years 2009-2020:

#### - Annual Actual Evpotranspiration and Interception (ETI<sub>a</sub>)
ETI<sub>a</sub> is the sum of the soil evaporation, canopy transpiration, and evaporation from rainfall intercepted by leaves (WaPOR Actual EvapoTranspiration and Interception (Annual), 2020). The units are in millimeters for a given year and the spatial resolution is at the 100m scale. The geographic coordinate system used is the WGS84 and the data is available for download in a raster format for 2009-2021. The data are from the Proba-V satellite until December 2019, at which this satellite was decommissioned and the data are replaced by the Copernicus Sentinel-2 mission.

#### - Annual Land Cover (LC)
LC is an experimental land cover dataset at the 100m scale using the WGS84 coordinate reference system (WaPO Land Cover Classification, 2020). The data are available for download in raster format for 2009-2020. The data is from MODIS using Copernicus training data. The land cover classification system was developed by the FAO and includes 25 possible classes.

#### - Annual Precipitation (PCP)
PCP is the total annual precipitation derived from summing daily data in units of millimeters (WaPOR Precipitation (Annual), 2021). This dataset is at the 5,000km scale using the WGS84 coordinate reference system and is available for download for the years 2009-2021, though daily or monthly data are updated approximately 5 days after observation. The source of the data is the Climate Hazards Group InfraRed Precipitation with Station (CHIRPS). This data is at the continental scale and only the Jordan River Basin is considered here.

### Two datasets are also used from HydroSHEDS for the purpose of mapping the basin:

#### - HydroRIVERS
The HydroRIVERS dataset is used to extract the Jordan River. The dataset is at 30m resolution and is downloadeable as a Geodatabase or Shapefile (HydroRIVERS, 2022). The data used for this map are derived from the Europe and Middle East Shapefile.

#### - HydroLAKES
HydroLAKES is used to extract the Tiberias Lake and Dead Sea along the Jordan River. The dataset is at varying resolution based on location and original data source and is downloadeable as a Geodatabase or Shapefile (HydroLAKES, 2016). The data used for this map are derived from the Lake polygons Shapefile.

## Description of Study Area
The Jordan River Basin is 41,000km<sup>2</sup> and spans five countries: west Jordan, east Israel, northeast Egypt, southeast Lebanon, and southwest Syria. Though the Jordan River itself does not reach Egypt, its basin extends into the northeast corner of the country. The Jordan River has headwaters on Mount Hermon and empties into the Dead Sea; the river serves as a border between Lebanon and Syria, Israel and Jordan, and the West Bank and Jordan. The river also travels through the Tiberias Lake in Israel. 

<p align="center">
  <img 
   src="https://github.com/moorec52/Jordan-WaPOR/blob/main/Images/BasinAreaPNG.PNG" 
   alt="Map of the Jordan River Basin and its Characteristics"
  >
</p>

## Analysis
Maps for the years 2009-2020 (inclusive) were downloaded from the FAO data portal and uploaded into ArcGIS Pro. The following steps were completed in the analysis:

1. Data processing
    - A basin polygon was created based on the 2009 ETI<sub>a</sub> raster.
    - Precipitation data was masked by the basin polygon.
    - NoData values were converted to null for all rasters using SetNull.
    - Data units were corrected using Raster Calculator for all rasters.
    - The symbology layer file was uploaded for the Land Cover and edited for Layer 2 properties.

2. Mosaic creation and initialization
    - Five multidimensional rasters were created in the form of a mosaic: Precipitation, ETI<sub>a</sub>, Land Cover, Land Cover for Cropland (divided by 3 cropland classifications), and Cropland (all three cropland classifications combined with no disctinction for simplification).
    - Mosaics were created using Create Mosaic Dataset.
    - Rasters were added using Add Rasters to Mosaic Dataset.
    - A field for "Year" was added for each raster in each mosaic, representing the Dimension. The ProductName field was updated based on data type for each raster in each mosaic, representing the Variable.
    - Mosaic multidimensional info was established using Build Multidimensional Info for each mosaic where the Variable is the ProductName and the Dimension is the Year.

3. Trend calculations
    - Trends for each datset were calculated: Precipitation, ETI<sub>a</sub>, and Cropland using  Generate Trend.
    - The output one-dimensional raster includes 5 bands across the basin area: Slope, Intercept, Root Mean Squared Error (RMSE), R<sup>2</sup> value, and the P-Value for each pixel.
    - Precipitation and ETI<sub>a</sub> trends were masked by the 2020 cropland area and statistics are computed for each band.

For the purpose of mapping, the HydroRIVERS and HydroLAKES data were uploaded and used as follows:

1. Data processing
    - Both datasets were masked by the basin polygon described above.
    - The HydroRIVERS dataset was filtered to streams with Strahler numbers of 4 or higher using Extract by Attributes.
    - The Jordan River was manually selected and exported.
    - The HydroLAKES dataset was manually filtered to select the Tiberias Lake and Dead Sea; these features were exported.

# Results

## Main Findings

### Cropland Area

Cropland area trends are not found to vary significantly, both across the study area as well as within individual pixels. The mean slope is 0.00, which corresponds to no change in cropland in the pixel. The confidence for this trend is very high, with an average R<sup>2</sup> value of 0.96. 
  <p align="center">
    <img 
     src="https://github.com/moorec52/Jordan-WaPOR/blob/main/Images/CroplandSlope.PNG" 
     alt="Map of the Jordan River Basin Cropland Area Trend Slope"
     width=400px
    >
    <img 
     src="https://github.com/moorec52/Jordan-WaPOR/blob/main/Images/CroplandR2.PNG" 
     alt="Map of the Jordan River Basin Cropland Area Trend Confidence"
     width=400px
    >
  </p>
  
  <p align="center">Map of cropland area slope (change per year, where 0 means no change and magnitude 1 means change every year) and R<sup>2</sup> values over the Jordan River Basin for 2009-2020.</p>
  
Due to little change in cropland over time, cropland area is assumed to not change for the purpose of this study, and cropland area is represented by the 2020 FAO map in the future analysis.
  <p align="center">
    <img
      src="https://github.com/moorec52/Jordan-WaPOR/blob/main/Images/LC2020.PNG" 
      alt="Map of the Jordan River Basin Cropland Area"
      width=400px
    >
    <img
      src="https://github.com/moorec52/Jordan-WaPOR/blob/main/Images/Cropland2020.PNG" 
      alt="Map of the Jordan River Basin Cropland Area"
      width=400px
    >
  </p>
  
  <p align="center">Map of land cover and cropland types in the Jordan River Basin in 2020.</p>
  
### Evapotranspiration and Infiltration

The trends for evapotranspiration and interception over time vary based on location in the river basin. The mean slope is 3.5 millimeters per year, with a standard deviation of 9.0 millimeters per year, indicating that the river basin on average is increasing in evapotranspiration and interception over time. However, this varies based on location, as shown in the map below. 

The mean slope for cropland is 2.2 millimeters per year with a standard deviation of 11.8 millimeters per year, so on average the cropland ETI<sub>a</a> is changing slower compared to the rest of the basin.

  <p align="center">
    <img 
     src="https://github.com/moorec52/Jordan-WaPOR/blob/main/Images/EITaSlope.PNG" 
     alt="Map of the Jordan River Basin EIT<sub>a</sub> Trend Slope"
     width=400px
    >
  </p>
  <p align="center">Map of change in EIT<sub>a</sub> (millimeters per year) over the Jordan River Basin for 2009-2020.</p>
  
The certianty of the trend equation also varies across pixels. The R<sup>2</sup> values range from 0-1, with a mean of 0.36 and a standard deviation of 0.26. The RMSE is also considered and is plotted below as percent error of average EIT<sub>a</sub> values over the time period. The square root of the RMSE is taken and divided by the average EIT<sub>a</sub> for each pixel. These values are relatively low, suggesting that this trend may still be applicable in some areas of the basin for certain applications depending on error sensitivity.

The R<sup>2</sup> mean value for cropland is lower than average, at 0.20 with a maximum of 0.98.

  <p align="center">
    <img 
     src="https://github.com/moorec52/Jordan-WaPOR/blob/main/Images/EITaR2.PNG" 
     alt="Map of the Jordan River Basin EIT<sub>a</sub> Trend R<sup>2</sup> values"
     width=400px
    >
    <img 
     src="https://github.com/moorec52/Jordan-WaPOR/blob/main/Images/EITaPercentError.PNG" 
     alt="Map of the Jordan River Basin EIT<sub>a</sub> Trend Percent Error"
     width=400px
    >
  </p>
  <p align="center">Map of R<sup>2</sup> and percent error values for EIT<sub>a</sub> from 2009-2010.</p>

### Precipitation



## Additional Graphics

# Discussion

# Conclusion

# References

FAO and IHE Delft. 2020. Water Accounting in the Jordan River Basin. FAO WaPOR water accounting reports. Rome. https://doi.org/10.4060/ca9181en.

FAO. 2020. FAO Water Productivity - Catalog. https://wapor.apps.fao.org/catalog/WAPOR_2/2.

Hydrolakes. HydroSHEDS. (2016, December). Retrieved April 24, 2022, from https://www.hydrosheds.org/products/hydrolakes.

HydroRIVERS. HydroSHEDS. (2022, April). Retrieved April 24, 2022, from https://www.hydrosheds.org/products/hydrorivers.

Sullivan, Caroline & Acreman, Mike & Wouters, Patricia & Castro, E. (2007). Facilitating adaptive management in large river basins: using the transboundary analytical framework. School of Environmental Science and Management Papers.

U.S. Department of Commerce and the European Commission and Swiss Administration. (n.d.). Jordan - Agricultural Sectors. Privacy Shield. Retrieved March 1, 2022, from https://www.privacyshield.gov/article?id=Jordan-AgriculturalSectors#:~:text=Agricultural%20exports%20have%20seen%20an,(both%20live%20and%20carcasses). 

United States Government. (2018, March 27). Blog: Mobilizing private-sector investment to transform Jordan's water system. Millennium Challenge Corporation. Retrieved March 1, 2022, from https://www.mcc.gov/blog/entry/blog-022217-mobilizing-private-sectorjordan.

WaPOR. (2020). Actual EvapoTranspiration and Interception (Annual). FAO Water Productivity. Retrieved March 1, 2022, from https://wapor.apps.fao.org/catalog/WAPOR_2/2/L2_AETI_A.

WaPOR. (2020). Land Cover Classification. FAO Water Productivity. Retrieved March 1, 2022, from https://wapor.apps.fao.org/catalog/WAPOR_2/2/L2_LCC_A.

WaPOR. (2021). Precipitation (Annual). FAO Water Productivity. Retrieved March 1, 2022, from https://wapor.apps.fao.org/catalog/WAPOR_2/1/L1_PCP_A.
