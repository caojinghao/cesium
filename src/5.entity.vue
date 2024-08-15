

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
        const position=Cesium.Cartesian3.fromDegrees(113.3191,23.109,2000)
        viewer.camera.flyTo({
          destination:position,
          orientation:{//默认（0，-90，0）
            heading:Cesium.Math.toRadians(0),
            pitch:Cesium.Math.toRadians(-90),
            roll:0,
          },
          duration:3
        })
        // 实体 entity
        // 写法1
        const point =new Cesium.Entity({
          position:Cesium.Cartesian3.fromDegrees(113.3191,23.109,800),
          point :{
            pixelSize:20,//像素大小
            color:Cesium.Color.BLUE,
            outlineWidth:4,
            outlineColor:Cesium.Color.RED

          }
        })
        viewer.entities.add(point)
        const label =viewer.entities.add({
          position:Cesium.Cartesian3.fromDegrees(113.3191,23.109,830),
            label:{
            text:'广州塔',
            font:'24px sans-serif',
            fillColor:Cesium.Color.WHITE,
            outlineColor:Cesium.Color.BLACK,
            outlineWidth:4,
            // fill 填充文字，outline勾勒标签，fill_AND_OUTLINE填充文字和勾勒标签
            style:Cesium.LabelStyle.FILL_AND_OUTLINE,
            // 设置文字的偏移量
            pixelOffset:new Cesium.Cartesian2(0,-24),
            // 设置文字的显示位置,LEFT RIGHT CENTER
            horizontalOrigin:Cesium.HorizontalOrigin.CENTER,
            // 设置文字垂直位置
            verticalOrigin:Cesium.VerticalOrigin.BOTTOM,
            // showBackground:true,
            // backgroundColor:new Cesium.Color(255,255,255)
          },
             billboard:{
                image:'/src/assets/gzt.png',
                // scale:0.1,
                width:50,
                height:50,
                verticalOrigin:Cesium.VerticalOrigin.BOTTOM,
                horizontalOrigin:Cesium.HorizontalOrigin.CENTER
                // color:Cesium.Color.YELLOWGREEN
          }
        })
        // 添加广州塔3D建筑 --使用cesium 自带模型
        const osmBuildings= viewer.scene.primitives.add(
          new Cesium.createOsmBuildings()
        )
        // viewer.zoomTo(point)
        

        // 写法2 推荐
      //  const point2=viewer.entities.add({
      //     id:'point',
      //     position:Cesium.Cartesian3.fromDegrees(121,30),
      //     point:{
      //       pixelSize:20,
      //       color:Cesium.Color.YELLOW
      //     }
      //   })


        // 标注
        // const billboard=viewer.entities.add({
        //   position:Cesium.Cartesian3.fromDegrees(116,40,10),
        //   billboard:{
        //     image:'/src/assets/position.jpeg',
        //     scale:0.1,
        //     color:Cesium.Color.YELLOWGREEN
        //   }
        // })

        // 文字
        // const label=viewer.entities.add({
        //   position:Cesium.Cartesian3.fromDegrees(112,30),
        //   label:{
        //     text:'Cesium',
        //     fillColor:Cesium.Color.AQUA,
        //     showBackground:true,
        //     backgroundColor:new Cesium.Color(255,255,255)
        //   }
        // })


        // 线
        // const polyline=viewer.entities.add({
        //   polyline:{
        //     positions:Cesium.Cartesian3.fromDegreesArray([120,20,121,20,121,20.5]),
        //     width:20,
        //     material:Cesium.Color.YELLOW
        //   }
        // })


        //面
        // const polygon=viewer.entities.add({
        //   polygon:{
        //     hierarchy:{
        //       positions:Cesium.Cartesian3.fromDegreesArray([120,25,121,25,121,25.5]),
        //     },
        //      // fill:true,
        //       material:Cesium.Color.YELLOW,
        //       height:100000,//高度
        //       extrudedHeight:200000,//拉伸
        //       outline:true,//是否显示外线
        //       outlineColor:Cesium.Color.WHITE,
        //      // fill:false, //是否填充
        //   }
        // })


        // 立方体
        // const box=viewer.entities.add({
        //   position:Cesium.Cartesian3.fromDegrees(119,30,30000),
        //   box:{
        //     dimensions:new Cesium.Cartesian3(2000,1000,3000),
        //     material:Cesium.Color.YELLOW
        //   }
        // })


        // // 椭圆
        // const ellipse=viewer.entities.add({
        //   position:Cesium.Cartesian3.fromDegrees(119,30),
        //   ellipse:{
        //     semiMajorAxis:500, //长轴
        //     semiMinorAxis:300,//短轴
        //     material:Cesium.Color.YELLOW,
        //     rotation:Math.PI/2,//旋转角
        //   }
        // })


        // // 矩形
        // const rectangle=viewer.entities.add({
        //   rectangle:{
        //     coordinates:Cesium.Rectangle.fromDegrees(120,40,123,45),
        //     extrudedHeight:300000,
        //     height:200000,
        //     material:"/src/assets/position.jpeg"
        //   }
        // })
        //  viewer.zoomTo(rectangle)
   
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

