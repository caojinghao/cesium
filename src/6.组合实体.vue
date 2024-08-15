

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
        infoBox:false, //右侧信息框
        selectionIndicator:false,//点几筐
        });
    //  const billboard=viewer.entities.add({
    //     position:Cesium.Cartesian3.fromDegrees(120,30,100),
    //     billboard:{
    //       image:"/src/assets/position.jpeg",
    //       scale: 0.1,
    //       color:Cesium.Color.Red
    //     }
    //   })
    //  const line=viewer.entities.add({
    //     polyline:{
    //       positions:Cesium.Cartesian3.fromDegreesArrayHeights([120,30,0,120,30,100]),
    //       material:Cesium.Color.AQUA
    //     }
    //  })
    //  const label=viewer.entities.add({
    //   position:Cesium.Cartesian3.fromDegrees(120,30,100),
    //   label:{
    //     text:'某某小区',
    //     font:'13px',
    //     fillColor:Cesium.Color.AQUAMARINE,
    //     pixelOffset:new Cesium.Cartesian2(0,-50)
    //   } 
    //  })
    //  组合实体
    const entity=viewer.entities.add({
      position:Cesium.Cartesian3.fromDegrees(120,30,100),
      billboard:{
          image:"/src/assets/position.jpeg",
          scale: 0.1,
          color:Cesium.Color.Red
        },
        polyline:{
          positions:Cesium.Cartesian3.fromDegreesArrayHeights([120,30,0,120,30,100]),
          material:Cesium.Color.AQUA
        },
        label:{
        text:'某某小区',
        font:'13px',
        fillColor:Cesium.Color.AQUAMARINE,
        pixelOffset:new Cesium.Cartesian2(0,-50)
      } 

    })
     viewer.zoomTo(entity)
   
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

