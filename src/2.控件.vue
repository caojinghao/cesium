

<template>
  <div id="cesiumContainer"></div>
</template>
<script setup>
import * as Cesium from 'cesium';
import { onMounted } from 'vue'; 
onMounted(() => { 

      Cesium.Ion.defaultAccessToken="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJlOTI0MzljZi0zMzkzLTRkM2QtODI3Zi0wZDZkMjZiZjkwMzIiLCJpZCI6MTg4ODg5LCJpYXQiOjE3MDQ4NzAwMTl9.r4bufkDRkVOKjKuR2kznbBNFk3oajDmhKlQmlIGNymA"
        const viewer = new Cesium.Viewer('cesiumContainer',{
         timeline:false, //时间轴
         animation:false,//动画控制
         geocoder:false,//搜索按钮
         homeButton:false,//主页按钮
         sceneModePicker:false,//投影方式按钮
         baseLayerPicker:false,//图层选择
        navigationHelpButton:false,//帮助手势
        fullscreenButton:false,//全屏按钮
        
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

.cesium-viewer-bottom{
  display: none;
}
</style>

