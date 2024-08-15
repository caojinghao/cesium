<template>
  <div id="cesiumContainer"></div>
  <button @click="todel">
    删除
  </button>
</template>
<script setup>
import * as Cesium from 'cesium';
import { onMounted } from 'vue';

let viewer, billboard, line, label, linePromise,polygonPromise

let arr = []
const todel = () => {
  // 直接删除
  // viewer.entities.remove(line)
  // viewer.entities.remove(label)
  // 删除所有
  // viewer.entities.removeAll()
  // id删除
  // viewer.entities.removeById('billboard')
  // 先拿后删
  // const entity=viewer.entities.getById('billboard')
  // viewer.entities.remove(entity)
  // 数组删除
  // arr.forEach((item)=>{
  //   viewer.entities.remove(item)
  // })
  console.log(viewer.dataSources, linePromise)
  viewer.dataSources.remove(polygonPromise)
}

onMounted(() => {
  Cesium.Ion.defaultAccessToken = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJlOTI0MzljZi0zMzkzLTRkM2QtODI3Zi0wZDZkMjZiZjkwMzIiLCJpZCI6MTg4ODg5LCJpYXQiOjE3MDQ4NzAwMTl9.r4bufkDRkVOKjKuR2kznbBNFk3oajDmhKlQmlIGNymA"
  viewer = new Cesium.Viewer('cesiumContainer', {
    timeline: false, //时间轴
    animation: false,//动画控制
    geocoder: false,//搜索按钮
    homeButton: false,//主页按钮
    sceneModePicker: false,//投影方式按钮
    baseLayerPicker: false,//图层选择
    navigationHelpButton: false,//帮助手势
    fullscreenButton: false,//全屏按钮
    infoBox: false, //右侧信息框
    selectionIndicator: false,//点几筐
  });
// 假设你已经获得了天地图的Key  
var key = '30f3fe9d57b4716cdc6811cd4b5d110d';  
var td_ken = '30f3fe9d57b4716cdc6811cd4b5d110d';  

  
// // 天地图影像图层的URL模板  
// var tdtImgUrlTemplate = 'https://t{s}.tianditu.gov.cn/DataServer?T=vec_w&x={x}&y={y}&l={z}&tk=' + key;  
  
// // 创建UrlTemplateImageryProvider实例  
// var imgProvider = new Cesium.UrlTemplateImageryProvider({  
//     url : tdtImgUrlTemplate,  
//     subdomains : ['0','1','2','3','4','5','6','7'],  
//     tilingScheme : new Cesium.WebMercatorTilingScheme(),  
//     maximumLevel : 18  
// });  
  
// // 假设你已经有了一个Cesium Viewer实例  
// viewer.imageryLayers.addImageryProvider(imgProvider);

//天地图 矢量地图
var td_vec_w = "http://{s}.tianditu.gov.cn/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
    "&LAYER=vec&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
    "&style=default&format=tiles&tk=" + td_ken;

var vecProvider = new Cesium.WebMapTileServiceImageryProvider({
    url: td_vec_w,
    layer: "vec_w",
    style: "default",
    format: "tiles",
    tileMatrixSetID: "image/jpeg",
    subdomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
    minimumLevel: 0,
    maximumLevel: 18
});


//天地图 矢量注记
var td_cva_w = "http://{s}.tianditu.gov.cn/cva_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
    "&LAYER=cva&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
    "&style=default&format=tiles&tk=" + td_ken;

var cvaProvider = new Cesium.WebMapTileServiceImageryProvider({
    url: td_cva_w,
    layer: "cva_w",
    style: "default",
    format: "tiles",
    tileMatrixSetID: "image/jpeg",
    subdomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
    minimumLevel: 0,
    maximumLevel: 18
});


//天地图 影像地图
var td_img_w = "http://{s}.tianditu.gov.cn/img_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
    "&LAYER=img&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
    "&style=default&format=tiles&tk=" + td_ken;

var imgProvider = new Cesium.WebMapTileServiceImageryProvider({
    url: td_img_w,
    layer: "img_w",
    style: "default",
    format: "tiles",
    tileMatrixSetID: "image/jpeg",
    subdomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
    minimumLevel: 0,
    maximumLevel: 18
});


//天地图 影像注记
var td_cia_w = "http://{s}.tianditu.gov.cn/cia_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
    "&LAYER=cia&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
    "&style=default&format=tiles&tk=" + td_ken;

var ciaProvider = new Cesium.WebMapTileServiceImageryProvider({
    url: td_cia_w,
    layer: "cia_w",
    style: "default",
    format: "tiles",
    tileMatrixSetID: "image/jpeg",
    subdomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
    minimumLevel: 0,
    maximumLevel: 18
});


//ArcGisMapServerImageryProvider 
var arcgisProvider = new Cesium.ArcGisMapServerImageryProvider({
    url: "https://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetPurplishBlue/MapServer",
    enablePickFeatures: false
});

//叠加多个影像图层
viewer.imageryLayers.addImageryProvider(vecProvider);
viewer.imageryLayers.addImageryProvider(cvaProvider);
viewer.imageryLayers.addImageryProvider(imgProvider);
viewer.imageryLayers.addImageryProvider(ciaProvider);
viewer.imageryLayers.addImageryProvider(arcgisProvider);

});

</script>
<style>
*,
body,
html {
  margin: 0;
  padding: 0;
}

body {
  margin: 0;
}

#app {
  width: 100vw;
  height: 100vh;
}

#cesiumContainer {
  width: 100vw;
  height: 100vh;
}

.cesium-viewer-bottom {
  display: none;
}

button {
  position: absolute;
  top: 10px;
  left: 10px;
  z-index: 999;
}
</style>
