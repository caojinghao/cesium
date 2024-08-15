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
    shouldAnimate:true,
  });
  // 添加单条线
  const linestring = turf.lineString([[115, -32], [131, -22], [143, -25], [150, -34],]);
  Cesium.GeoJsonDataSource.load(linestring).then(res => {
    const entity = res.entities.values[0]
    viewer.entities.add(entity)
    // viewer.zoomTo(entity)
  })
  // 添加多条线
  const multiline = turf.multiLineString([[[110, -32], [110, -22]], [[116, -32], [132, -22]]]);
  line= Cesium.GeoJsonDataSource.load(multiline).then(res=>{
    viewer.dataSources.add(res)
    // viewer.zoomTo(res)
  })

  // console.log(linePromise)
  // viewer.dataSources.add(linePromise)
  // viewer.zoomTo(linePromise)
   // 添加面
  // const polygon = turf.polygon([[[-5, 52],[-4, 56], [-7, 54], [-5, 52],],],);
  // polygonPromise = Cesium.GeoJsonDataSource.load(polygon)
  // viewer.dataSources.add(polygonPromise)
  // viewer.zoomTo(polygonPromise)
  //  viewer.zoomTo(line)

    // 添加线、面 topojson geojson
    // linePromise = Cesium.GeoJsonDataSource.load('/src/assets/line.topojson')
    // linePromise = Cesium.GeoJsonDataSource.load('/src/assets/line.geojson')

    //面 topojson geojson
    // polygonPromise = Cesium.GeoJsonDataSource.load('/src/assets/polygon.topojson')
    // polygonPromise = Cesium.GeoJsonDataSource.load('/src/assets/polygon.geojson',{
    //   stroke:Cesium.Color.RED,
    //   fill:Cesium.Color.SKYBLUE.withAlpha(0.5),
    //   strokeWidth:4,
    // })
    // polygonPromise.then((res)=>{
    //   viewer.dataSources.add(res)
    //   let entities=res.entities.values
    //   entities.forEach((item,index)=>{
    //     item.polygon.material=new Cesium.ColorMaterialProperty(
    //       Cesium.Color.fromRandom({
    //         alpha:0.9
    //       })
    //     )
    //     item.polygon.outline=false
    //     item.polygon.extrudedHeight=1000
    //   })
    // })
    // 加载kml数据
    // polygonPromise = Cesium.KmlDataSource.load('/public/Assets/gdpPerCapita2008.kmz')
    // viewer.dataSources.add(polygonPromise)
    // viewer.zoomTo(polygonPromise)
    // 加载kml数据
  const czml = [
    {
      id: "document",
      name: "CZML Point - Time Dynamic",
      version: "1.0",
    },
    {
      id: "point",
      // 物体在什么时间范围可用
      availability: "2012-08-04T16:00:00Z/2012-08-04T16:05:00Z",
      position: {
        // 设置物体的起始时间
        epoch: "2012-08-04T16:00:00Z",
        // 设置了四个维度，1维是时间，2维是经度，3维是纬度，4维是高度
        cartographicDegrees: [
          0, -70, 20, 150000, 100, -80, 44, 150000, 200, -90, 18, 150000, 300,
          -98, 52, 150000,
        ],
      },
      point: {
        color: {
          rgba: [255, 255, 255, 128],
        },
        outlineColor: {
          rgba: [255, 0, 0, 128],
        },
        outlineWidth: 3,
        pixelSize: 15,
      },
    },
  ];

  // 加载czml数据
  let promiseData = Cesium.CzmlDataSource.load(czml);
  promiseData.then((dataSource) => {
    console.log(dataSource);
    viewer.dataSources.add(dataSource);
    // viewer.flyTo(dataSource);
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
