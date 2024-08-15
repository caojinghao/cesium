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
    skyBox:new Cesium.SkyBox({ //设置天空盒
      sources:{
        positiveX:"/public/texture/sky/px.jpg",
        negativeX:"/public/texture/sky/nx.jpg",
        positiveY:"/public/texture/sky/ny.jpg",
        negativeY:"/public/texture/sky/py.jpg",
        positiveZ:"/public/texture/sky/pz.jpg",
        negativeZ:"/public/texture/sky/nz.jpg",
      }
    //   sources : {  
    //     positiveX : Cesium.buildModuleUrl('Assets/Textures/skyBox/brightblue_px.jpg'),  
    //     negativeX : Cesium.buildModuleUrl('Assets/Textures/skyBox/brightblue_nx.jpg'),  
    //     positiveY : Cesium.buildModuleUrl('Assets/Textures/skyBox/brightblue_py.jpg'),  
    //     negativeY : Cesium.buildModuleUrl('Assets/Textures/skyBox/brightblue_ny.jpg'),  
    //     positiveZ : Cesium.buildModuleUrl('Assets/Textures/skyBox/brightblue_pz.jpg'),  
    //     negativeZ : Cesium.buildModuleUrl('Assets/Textures/skyBox/brightblue_nz.jpg')  
    // } 
    })
  });
  // 1、其中，longitudeMin、latitudeMin、longitudeMax、latitudeMax分别代表矩形区域的左下角和右上角的经纬度。例如，要设置默认中心点为中国上空，可以这样做：
  // Cesium.Camera.DEFAULT_VIEW_RECTANGLE = Cesium.Rectangle.fromDegrees(80, 22, 130, 55);

  // 2、例如，将默认中心点设置为北京的经纬度，并设置一定的初始高度和视角：
  viewer.camera.setView({  
    destination: Cesium.Cartesian3.fromDegrees(116.407396, 39.904200, 120000.0),  
    orientation: {  
        heading: Cesium.Math.toRadians(0.0),  
        pitch: Cesium.Math.toRadians(-90.0), // 垂直向下看  
        roll: 0.0  
    }  
});
// 假设你已经获得了天地图的Key  
var key = '30f3fe9d57b4716cdc6811cd4b5d110d';  

  
// 天地图影像图层的URL模板  添加多个
var tdtImgUrlTemplate = 'https://t{s}.tianditu.gov.cn/DataServer?T=vec_w&x={x}&y={y}&l={z}&tk=' + key;  
var tdtImgUrlTemplate2 = 'https://t{s}.tianditu.gov.cn/DataServer?T=cia_w&x={x}&y={y}&l={z}&tk=' + key;  
  
// 创建UrlTemplateImageryProvider实例  
var imgProvider = new Cesium.UrlTemplateImageryProvider({  
    url : tdtImgUrlTemplate,  
    subdomains : ['0','1','2','3','4','5','6','7'],  
    tilingScheme : new Cesium.WebMercatorTilingScheme(),  
    maximumLevel : 18  
}); 
var imgProvider2= new Cesium.UrlTemplateImageryProvider({  
    url : tdtImgUrlTemplate2,  
    subdomains : ['0','1','2','3','4','5','6','7'],  
    tilingScheme : new Cesium.WebMercatorTilingScheme(),  
    maximumLevel : 18  
});   
  
// 假设你已经有了一个Cesium Viewer实例  
 var layer=viewer.imageryLayers.addImageryProvider(imgProvider);
 layer.alpha=0.8
viewer.imageryLayers.addImageryProvider(imgProvider2);

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
