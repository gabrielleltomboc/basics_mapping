# basics_mapping
// larger rectangle
var weather = ee.ImageCollection("NASA/ORNL/DAYMET_V4")
  .filterBounds(palmyra)
  .filterDate('2024-07-01', '2024-07-07')
  .mean();
  
  var tmax = weather.select('tmax');
  
  var tmaxVis = {
    min: 15,
    max: 35,
    palette: ['blue', 'white', 'red']
  };
  
  Map.addLayer(tmax, tmaxVis, 'Daymet Max Temp');
  
  var weatherCombined = weather.select(['vp', 'tmax', 'prcp']);
  
  var weatherCombinedVis = {
    bands: ["vp", "tmax", "prcp"],
    min: [500, 15, 0],
    max: [3000, 35, 10]
  };
  
  Map.addLayer(weatherCombined, weatherCombinedVis, 'Information Combined');
  
Export.image.toDrive({
  image: weatherCombined,        // raw bands, not visualized RGB
  description: 'Daymet_vp_tmax_prcp_raw',
  region: region,
  scale: 1000,
  crs: 'EPSG:4326',
  fileFormat: 'GeoTIFF',
  maxPixels: 1e13
});


Export.image.toDrive({
  image: exportRGB,
  description: 'fcRGB_Daymet_vp_tmax_prcp_2024Jul',
  region: region, 
  scale: 1000,
  crs: 'EPSG:4326',
  fileFormat: 'GeoTIFF',
  maxPixels: 1e13
});

<a>https://code.earthengine.google.com/<a>
