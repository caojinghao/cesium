<template>
  <div id="cesiumContainer"></div>
  <button @click="todel">
    删除
  </button>
</template>
<script setup>
import * as Cesium from 'cesium';
import { onMounted } from 'vue';
import gzt from "../public/texture/Fire.png"
import gsap from 'gsap'
let viewer, billboard, line, label, linePromise, polygonPromise

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
  // entity 和 primitive区别
  // entity是由 primitive 封装的使用更简洁
  //primitive更接近webgl底层，可以绘制更高级的图形
  // geometry(几何形状) +appearance(外观)构成primitive
  // 1. Draw a translucent ellipse on the surface with a checkerboard pattern
  //   const primitive =new Cesium.Primitive({
  //     geometryInstances:new Cesium.GeometryInstance({
  //       geometry : new Cesium.EllipseGeometry({
  //           center : Cesium.Cartesian3.fromDegrees(-100.0, 20.0),
  //           semiMinorAxis : 500000.0,
  //           semiMajorAxis : 1000000.0,
  //           rotation : Cesium.Math.PI_OVER_FOUR,
  //           vertexFormat : Cesium.VertexFormat.POSITION_AND_ST
  //       }),
  //     }),
  //     appearance : new Cesium.EllipsoidSurfaceAppearance({
  //       // material : Cesium.Material.fromType('Checkerboard'),
  //       material:new Cesium.Material({
  //         fabric:{
  //           type:'Color',
  //           uniforms:{
  //             color:new Cesium.Color(1.0,0.0,0.0,0.5)
  //           }
  //         }
  //       })
  //     })
  //   });
  //   viewer.scene.primitives.add(primitive)

  // 2. Draw different instances each with a unique color

  const rectangleInstance = new Cesium.GeometryInstance({
    id: 'blueRect',
    geometry: new Cesium.RectangleGeometry({
      rectangle: Cesium.Rectangle.fromDegrees(-140.0, 30.0, -100.0, 40.0),
    }),
    attributes: {
      color: new Cesium.ColorGeometryInstanceAttribute(0.0, 1.0, 1.0, 0.5)
    },
    vertexFormat: Cesium.EllipsoidSurfaceAppearance.VERTEX_FORMAT
  });

  const polygonInstance = new Cesium.GeometryInstance({
    id: 'blueRect1',
    geometry: new Cesium.PolygonGeometry({
      polygonHierarchy: new Cesium.PolygonHierarchy(
        Cesium.Cartesian3.fromDegreesArray([
          -100.0, 50.0,
          -60.0, 30.0,
          -100.0, 20.0,
        ])
      )
    }),
    attributes: {
      color: new Cesium.ColorGeometryInstanceAttribute(0.0, 1.0, 1.0, 0.5)
    },
    vertexFormat: Cesium.EllipsoidSurfaceAppearance.VERTEX_FORMAT
  });

  // type color
  // let material3=new Cesium.Material.fromType("Color",{
  //   color:Cesium.Color.RED.withAlpha(0.5)
  // })

  // type image
  // let material3 = new Cesium.Material.fromType("Image", {
  //   image: gzt,
  //   repeat: new Cesium.Cartesian2(2.0, 2.0)
  // })

  // type disffuseMap
  // let material3=new Cesium.Material.fromType("DiffuseMap",{
  //   image:gzt,
  // })

  // type grid
  // let material3 = new Cesium.Material.fromType("Grid", {
  //   color: Cesium.Color.AQUA.withAlpha(0.5),
  //   cellAlpha: 0.2,
  //   lineCount: new Cesium.Cartesian2(4, 4),
  //   lineThickness: new Cesium.Cartesian2(4.0, 4.0),
  // });

  // type water 水面效果
  // let material3 = new Cesium.Material.fromType("Water", {
  //   baseWaterColor: Cesium.Color.AQUA.withAlpha(0.7),
  //   distortion: 0.25,
  //   normalMap: "/public/texture/waterNormals.jpg",
  // });

  // 使用fabric 方式
  // let material3=new Cesium.Material({
  //   fabric:{
  //     type:"Image",
  //     uniforms:{
  //       image:"/public/texture/logo.png"
  //     }
  //   }
  // })
  

  // 编写着色器 修改材质 动态的
  let material3= new Cesium.Material({
    fabric: {
      uniforms: {
        uTime: 0,
      },
      source: `
        czm_material czm_getMaterial(czm_materialInput materialInput)
        {
          // 生成默认的基础材质
          czm_material material = czm_getDefaultMaterial(materialInput);
          // material.diffuse = vec3(materialInput.st+uTime, 0.0);
          float strength = mod((materialInput.s-uTime) * 10.0, 1.0);
          material.diffuse = vec3(strength, 0.0, 0.0);
          return material;
        }
      `,
    },
  })

  gsap.to(material3.uniforms, {
    uTime: 1,
    duration: 2,
    repeat: -1,
    ease: "linear",  //线性效果
  });
  let appearance = new Cesium.EllipsoidSurfaceAppearance({
    material: material3,
    aboveGround: false
  })

  // 设置外观 使用instance的颜色去着色
  // let appearance= new Cesium.PerInstanceColorAppearance({
  // flat:true
  // })


  // 设置图元
  let primitive = new Cesium.Primitive({
    geometryInstances: [rectangleInstance, polygonInstance],
    appearance: appearance
  })

  // 把添加的数据加到地图上
  viewer.scene.primitives.add(primitive);

  // let material=new Cesium.ColorMaterialProperty(
  //   new Cesium.Color(1.0,1.0,1.0,1.0)
  // )

  // 棋盘纹理材质
  // let material=new Cesium.CheckerboardMaterialProperty({
  //   evenColor: Cesium.Color.WHITE,
  //   oddColor: Cesium.Color.BLACK,
  //   repeat: new Cesium.Cartesian2(2, 2)
  // })

  // 条纹材质
  // let material=new Cesium.StripeMaterialProperty({
  //   evenColor: Cesium.Color.WHITE,
  //   oddColor: Cesium.Color.BLACK,
  //   repeat: 23
  // })

  // 网格纹理
  let material = new Cesium.GridMaterialProperty({
    color: Cesium.Color.YELLOW,
    cellAlpha: 0.2,
    lineCount: new Cesium.Cartesian2(8, 8),
    lineThickness: new Cesium.Cartesian2(2.0, 2.0),
  });

  // 使用entity 创建矩形
  const rectangle = viewer.entities.add({
    id: 'entityID',
    rectangle: {
      coordinates: Cesium.Rectangle.fromDegrees(-150, 20, -100, 30),
      // extrudedHeight:300000,
      // height:200000,
      // ColorMaterialProperty材质
      material: material
      // material:Cesium.Color.RED.withAlpha(0.5)
      // material:"/src/assets/position.jpeg"
    }
  })
  viewer.zoomTo(rectangle)

  // 设置虚线材质
  // let material2=new Cesium.PolylineDashMaterialProperty({
  //   dashLength:10,
  //   color:Cesium.Color.RED
  // })

  // 设置箭头材质
  // let material2=new Cesium.PolylineArrowMaterialProperty(Cesium.Color.RED)

  // 设置发光飞线效果
  let material2 = new Cesium.PolylineGlowMaterialProperty({
    // 设置发光程度
    glowPower: 0.1,
    // 设置尾椎程度
    taperPower: 0.7,
    color: Cesium.Color.RED
  })
  // 创建线，
  const polyline = viewer.entities.add({
    polyline: {
      positions: Cesium.Cartesian3.fromDegreesArray([-150, 10, -100, 10]),
      width: 40,
      material: material2
      // material:Cesium.Color.YELLOW
    }
  })
  // 拾取 鼠标事件
  var handler = new Cesium.ScreenSpaceEventHandler(viewer.scene.canvas)
  handler.setInputAction((movement) => {
    console.log(movement)
    //scene.pick 选中物体
    var pickObject = viewer.scene.pick(movement.position)
    console.log(pickObject)
    if (Cesium.defined(pickObject) && typeof (pickObject.id) == "string") {
      let attributes = primitive.getGeometryInstanceAttributes(pickObject.id)
      attributes.color = Cesium.ColorGeometryInstanceAttribute.toValue(
        Cesium.Color.YELLOW.withAlpha(0.5)
      )
    }
  }, Cesium.ScreenSpaceEventType.LEFT_CLICK)



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
