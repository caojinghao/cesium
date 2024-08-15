

<template>
  <div id="cesiumContainer"></div>
</template>
<script setup>
import * as Cesium from 'cesium';
import { onMounted } from 'vue'; 
onMounted(() => { 
  // ArcGIS 影像图层
  const esri= new Cesium.ArcGisMapServerImageryProvider({
    url:'https://services.arcgisonline.com/ArcGIS/rest/services/World_Imagery/Mapserver',
    enablePickFeatures:false
  })
      Cesium.Ion.defaultAccessToken="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJlOTI0MzljZi0zMzkzLTRkM2QtODI3Zi0wZDZkMjZiZjkwMzIiLCJpZCI6MTg4ODg5LCJpYXQiOjE3MDQ4NzAwMTl9.r4bufkDRkVOKjKuR2kznbBNFk3oajDmhKlQmlIGNymA"
        const viewer = new Cesium.Viewer('cesiumContainer',{
          imageryProvider:esri,
          terrainProvider:Cesium.createWorldTerrain({
            requestWaterMask:true//水面
          })//地形图层
        });
 }); 

</script>
<style>
*,body,html{
  margin: 0;
  padding: 0;
}
body{
  margin: 0;
}
#app{
  width: 100vw;
  height: 100vh;
}
#cesiumContainer{
  width: 100vw;
  height: 100vh;
}
</style>

