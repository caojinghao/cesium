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
    infoBox: true, //右侧信息框
    selectionIndicator: false,//点几筐
  });
// 添加3D建筑
let tiles3d = new Cesium.createOsmBuildings();
  const osmBuildings = viewer.scene.primitives.add(tiles3d);
  // 广州塔
  var postion = Cesium.Cartesian3.fromDegrees(
    // 经度
    113.3191,
    // 纬度
    23.109,
    // 高度
    1000
  );
  viewer.camera.flyTo({
    destination: postion,
    duration: 2,
  });

    // console.log(tiles3d);
  tiles3d.style = new Cesium.Cesium3DTileStyle({
    // 颜色设置，颜色名称/16进制颜色值/rgba颜色值
    // color: "color('yellow')",
    color: "rgba(255, 255, 0, 0.5)",
    // color: "color('#f00')",
    show: true,
  });
  tiles3d.style = new Cesium.Cesium3DTileStyle({
    color: {
      conditions: [
        [
          "${feature['building']} === 'apartments'",
          "color('rgba(50, 255, 0, 0.5)')",
        ],
        [
          "${feature['building']} === 'office'",
          "color('rgba(255, 255, 0, 0.5)')",
        ],
        [
          "${feature['cesium#estimatedHeight']} > 300",
          "color('rgba(200, 200, 255, 0.7)')",
        ],
        [
          "${feature['cesium#estimatedHeight']} > 100",
          "color('rgba(100, 100, 255, 0.7)')",
        ],
        [
          "${feature['cesium#estimatedHeight']} > 50",
          "color('rgba(50, 50, 150, 0.7)')",
        ],
        ["true", "color('white')"],
      ],
    },
    show: true,
  });
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
