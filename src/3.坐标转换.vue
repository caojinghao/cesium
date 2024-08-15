

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
        // 坐标系分类
        // 1.屏幕坐标系，二维笛卡尔坐标系，Cartesian2类型
        // 2.地理坐标系，WGS-84坐标系，Cartographic 类型，经度，纬度，高度
        // 3.笛卡尔空间直角坐标系，Caresian3类型

        // 角度与弧度转换
        var radians=Cesium.Math.toRadians(90)
        // 弧度转角度
        var degrees=Cesium.Math.toDegrees(2* Math.PI)
        // 经纬度

      // 返回一个笛卡尔坐标
      const  cartesian=  Cesium.Cartesian3.fromDegrees(110,20,20) //经度纬度高度
        //  笛卡尔转弧度坐标转经纬度
        // 返回弧度坐标
      const cartographic= Cesium.Cartographic.fromCartesian(cartesian)
      // 弧度转经纬度
      // let lon= 180 / Math.PI * cartographic.longitude
      // let lat= 180 / Math.PI * cartographic.latitude
      let lon=Cesium.Math.toDegrees(cartographic.longitude)
      let lat=Cesium.Math.toDegrees(cartographic.latitude)
   console.log(cartesian,lon,lat,cartographic.height)
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

