

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
        // viewer.camera.setView
        const position =Cesium.Cartesian3.fromDegrees(110,20,30000)

        // setView 通过定义相机目的地，直接跳转到目的地 瞬间到达指定位置，指定视角
        // viewer.camera.setView({
        //   destination:position,
        //   orientation:{//默认（0，-90，0）
        //     heading:Cesium.Math.toRadians(0),//指定朝向 0是正北
        //     pitch:Cesium.Math.toRadians(0), //指定俯仰角 0度竖直向上，-90度是竖直向下
        //     roll:Cesium.Math.toRadians(0), //指定滚动角，翻滚角，歪头角
        //   }
        // })

        // // flyTo 快速切换视角，带飞行动画，可以设置飞行时长
        // viewer.camera.flyTo({
        //   destination:position,
        //   orientation:{//默认（0，-90，0）
        //     heading:Cesium.Math.toRadians(0),
        //     pitch:Cesium.Math.toRadians(0),
        //     roll:Cesium.Math.toRadians(0),
        //   },
        //   duration:100
        // })

        // lookAt 将视角固定在设置的点位上，可以选择视角，但不能改变位置
        // const position2=Cesium.Cartesian3.fromDegrees(110,20)
        // viewer.camera.lookAt(
        //   position2,
        //   new Cesium.HeadingPitchRange(
        //     Cesium.Math.toRadians(0),
        //     Cesium.Math.toRadians(-90),
        //     2000
        //   )
        // )

        // 通过按键移动相机
        document.addEventListener('keydown',(e)=>{
          // 获取相机离地面的高度
          var height=viewer.camera.positionCartographic.height
          var moveRate=height/100
          if(e.key=='w'){
            // 设置相机向前移动
            viewer.camera.moveForward(moveRate)
          }else if(e.key=='s'){
            // 设置相机向后移动
            viewer.camera.moveBackward(moveRate)
          }else if(e.key=='a'){
            // 设置相机向左移动
            viewer.camera.moveLeft(moveRate)
          }else if(e.key=="d"){
            // 设置相机向右移动
            viewer.camera.moveRight(moveRate)
          }else if(e.key=="q"){
            // 设置相机向左旋转
            viewer.camera.lookLeft(Cesium.Math.toRadians(0.1))
          }else if(e.key=="e"){
            // 设置相机向右旋转
            viewer.camera.lookRight(Cesium.Math.toRadians(0.1))
          }else if(e.key=="z"){
            // 设置相机向上旋转
            viewer.camera.lookUp(Cesium.Math.toRadians(0.1))
          }else if(e.key=="c"){
            // 设置相机向下旋转
            viewer.camera.lookDown(Cesium.Math.toRadians(0.1))
          }else if(e.key=="g"){
            // 设置相机向左逆时针翻滚
            viewer.camera.twistLeft(1)
          }else if(e.key=="h"){
            // 设置相机向右逆时针翻滚
            viewer.camera.twistRight(Cesium.Math.toRadians(1))
          }

        })

   
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

