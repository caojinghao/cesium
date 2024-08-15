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
  // const tileset = viewer.scene.primitives.add(
  //   new Cesium.Cesium3DTileset({
  //     url:Cesium.IonResource.fromAssetId(69380) //69380 75343
  //   })
  // ); 
  // viewer.flyTo(tileset)
  const tileset= new Cesium.Cesium3DTileset({
    url:'/public/Assets/tileset.json'
  })
  viewer.scene.primitives.add(tileset)

  tileset.readyPromise.then((tileset)=>{
    viewer.zoomTo(tileset)
  })

  // 性能监控
  viewer.extend(Cesium.viewerCesium3DTilesInspectorMixin)
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
