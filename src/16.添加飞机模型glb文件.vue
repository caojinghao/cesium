<template>
  <div id="cesiumContainer"></div>
</template>
<script setup>
import * as Cesium from 'cesium';
import { onMounted } from 'vue';
import planeData from "./assets/json/plane.json"
onMounted(() => {

  Cesium.Ion.defaultAccessToken = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJlOTI0MzljZi0zMzkzLTRkM2QtODI3Zi0wZDZkMjZiZjkwMzIiLCJpZCI6MTg4ODg5LCJpYXQiOjE3MDQ4NzAwMTl9.r4bufkDRkVOKjKuR2kznbBNFk3oajDmhKlQmlIGNymA"
  const viewer = new Cesium.Viewer('cesiumContainer', {
    timeline: true, //时间轴
    animation: true,//动画控制
    geocoder: false,//搜索按钮
    homeButton: false,//主页按钮
    sceneModePicker: false,//投影方式按钮
    baseLayerPicker: false,//图层选择
    navigationHelpButton: false,//帮助手势
    fullscreenButton: false,//全屏按钮
    terrainProvider: Cesium.createWorldTerrain(
      {
        requestWaterMask: true,
        requestVertexNormals: true
      },
    )
  });
  // const position=Cesium.Cartesian3.fromDegrees(113.3191,23.109,2000)
  // viewer.camera.flyTo({
  //   destination:position,
  //   orientation:{//默认（0，-90，0）
  //     heading:Cesium.Math.toRadians(0),
  //     pitch:Cesium.Math.toRadians(-90),
  //     roll:0,
  //   },
  //   duration:3
  // })

  // 添加广州塔3D建筑 --使用cesium 自带模型
  const osmBuildings = viewer.scene.primitives.add(
    new Cesium.createOsmBuildings()
  )
  // 添加3D模型
  // const airplane=viewer.entities.add({
  //   name:'Airplane',
  //   position:Cesium.Cartesian3.fromDegrees(113.3191,23.109,1000),
  //   model:{
  //     uri:'/public/model/Air.glb',
  //     // 设置飞机的最小像素,设置之后模型始终保持不变
  //     minimumPixelSize:128,
  //     // runAnimations:true,
  //     // 设置飞机的轮廓
  //     silhouetteSize:30,
  //     // 设置轮廓的颜色
  //     silhouetteColor:Cesium.Color.WHITE,
  //     // 设置相机距离模型多远的距离显示 单位 M
  //     distanceDisplayCondition:new Cesium.DistanceDisplayCondition(0,200000)
  //   }
  // })
  // viewer.zoomTo(point)

  console.log(planeData)
  const positionProperty = new Cesium.SampledPositionProperty()

  // 时间间隔 30
  const timeStepInSeconds = 30
  // 整个飞行花费的时间
  const totalSeconds = (planeData.length - 1) * timeStepInSeconds

  // 设置起点时间
  const time = new Date("2022-03-09T23:10:00Z")

  // cesium ,默认使用的是儒略日的时间
  // 所以需要把时间改为儒略日的时间
  const startJulianDate = Cesium.JulianDate.fromDate(time)
  // 设置终点时间
  const stopJulianDate = Cesium.JulianDate.addSeconds(
    startJulianDate,
    totalSeconds,
    new Cesium.JulianDate()
  )


  // 将查看器的时间调整到起点和结束点的范围
  viewer.clock.startTime = startJulianDate.clone()
  viewer.clock.stopTime = stopJulianDate.clone()
  viewer.clock.currentTime = startJulianDate.clone()

  viewer.timeline.zoomTo(startJulianDate, stopJulianDate)

  planeData.forEach((dataPoint, i) => {
    const time = Cesium.JulianDate.addSeconds(
      startJulianDate,
      i * timeStepInSeconds,
      new Cesium.JulianDate()
    )
    const position = Cesium.Cartesian3.fromDegrees(
      dataPoint.longitude,
      dataPoint.latitude,
      dataPoint.height
    )
    // 添加轨迹采样点
    positionProperty.addSample(time, position)

    //添加点数据
    viewer.entities.add({
      position: position,
      point: {
        pixelSize: 10,
        color: Cesium.Color.RED,
        outlineColor: Cesium.Color.WHITE,
        outlineWidth: 2,
      }
    })
    // viewer.zoomTo
  })


  // 创建飞机
  const airplane = viewer.entities.add({
    // 创建飞机可显示的时间集合
    availability:new Cesium.TimeIntervalCollection([
      new Cesium.TimeInterval({
        start:startJulianDate,
        stop:stopJulianDate
      })
    ]),
    name: 'Airplane',
    id:'billboard',
    position: positionProperty,
    // VelocityOrientationProperty 会自动根据采样点，计算出飞机的速度和方向
    orientation: new Cesium.VelocityOrientationProperty(positionProperty),
    model: {
      uri: '/public/model/Air.glb',
      // 设置飞机的最小像素,设置之后模型始终保持不变
      minimumPixelSize: 128,
      maximumScale: 20000,
      // runAnimations:true,
      // 设置飞机的轮廓
      // silhouetteSize: 30,
      // 设置轮廓的颜色
      // silhouetteColor: Cesium.Color.WHITE,
      // 设置相机距离模型多远的距离显示 单位 M
      // distanceDisplayCondition:new Cesium.DistanceDisplayCondition(0,200000)
    },
    // 绘制轨迹线
    path: new Cesium.PathGraphics({
      with: 5
    })
  })

  // 相机追踪运动的物体
  viewer.trackedEntity=airplane

  // 设置时间速率
  viewer.clock.multiplier=60
  // setTimeout(() => {
  //   viewer.trackedEntity=''
  // }, 10000);
  // setTimeout(() => {
  //   viewer.trackedEntity=viewer.entities.getById('billboard')
  // }, 15000);
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
</style>
