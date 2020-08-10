<template>
  <div id="cesiumContainer">
    <div id="trackPopUp">
      <div id="trackPopUpContent" class="leaflet-popup" style="top:5px;left:0;z-index:1;">
        <a class="leaflet-popup-close-button" href="#">×</a>
        <div class="leaflet-popup-content-wrapper">
          <div id="trackPopUpLink" class="leaflet-popup-content" style="max-width: 300px;"></div>
        </div>
        <div class="leaflet-popup-tip-container">
          <div class="leaflet-popup-tip"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "cesiumContainer",
  mounted() {
    let that = this;

    //天地图URL配置
    let URL_CONFIG = {
      TDT_IMG_W:
        "http://{s}.tianditu.gov.cn/img_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=img&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7", //在线天地图影像服务地址(墨卡托投影)
      TDT_VEC_W:
        "http://{s}.tianditu.gov.cn/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=vec&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7", //在线天地图矢量地图服务(墨卡托投影)
      TDT_CIA_W:
        "http://{s}.tianditu.gov.cn/cia_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=cia&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default.jpg&tk=ca9c419b9135b2179d0ff37413bba8b7", //在线天地图影像中文标记服务(墨卡托投影)
      TDT_CVA_W:
        "http://{s}.tianditu.gov.cn/cva_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=cva&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default.jpg&tk=ca9c419b9135b2179d0ff37413bba8b7", //在线天地图矢量中文标记服务(墨卡托投影)
      TDT_IMG_C:
        "http://{s}.tianditu.gov.cn/img_c/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=img&tileMatrixSet=c&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7", //在线天地图影像服务地址(经纬度)
      TDT_VEC_C:
        "http://{s}.tianditu.gov.cn/vec_c/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=vec&tileMatrixSet=c&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7", //在线天地图矢量地图服务(经纬度)
      TDT_CIA_C:
        "http://{s}.tianditu.gov.cn/cia_c/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=cia&tileMatrixSet=c&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7", //在线天地图影像中文标记服务(经纬度)
      TDT_CVA_C:
        "http://{s}.tianditu.gov.cn/cva_c/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=cva&tileMatrixSet=c&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7", //在线天地图矢量中文标记服务(经纬度)
    };

    //buildModuleUrl.setBaseUrl('../../static/Cesium/')
    debugger;
    // eslint-disable-next-line no-undef
    that.viewer = new Cesium.Viewer("cesiumContainer", {
      animation: false,
      baseLayerPicker: false,
      fullscreenButton: false,
      geocoder: false,
      homeButton: false,
      sceneModePicker: true,
      selectionIndicator: false,
      timeline: false,
      navigationHelpButton: false,
      infoBox: false,
      navigationInstructionsInitiallyVisible: false,
      //天地图影像服务（经纬度）
      // eslint-disable-next-line no-undef
      imageryProvider: new Cesium.WebMapTileServiceImageryProvider({
        url: URL_CONFIG.TDT_IMG_C,
        layer: "tdtImg_c",
        style: "default",
        format: "tiles",
        tileMatrixSetID: "c",
        subdomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
        // eslint-disable-next-line no-undef
        tilingScheme: new Cesium.GeographicTilingScheme(),
        tileMatrixLabels: [
          "1",
          "2",
          "3",
          "4",
          "5",
          "6",
          "7",
          "8",
          "9",
          "10",
          "11",
          "12",
          "13",
          "14",
          "15",
          "16",
          "17",
          "18",
        ],
        maximumLevel: 18,
        show: false,
      }),
      // terrainProvider: new Cesium.CesiumTerrainProvider({
      //   url: "http://192.168.3.121/tileServer/China_Terrain/",
      //   // 请求照明
      //   requestVertexNormals: true,
      //   // 请求水波纹效果
      //   requestWaterMask: true
      // })
    });

    // eslint-disable-next-line no-undef
    that.viewer.imageryLayers.addImageryProvider(
      // eslint-disable-next-line no-undef
      new Cesium.WebMapTileServiceImageryProvider({
        //调用影响中文注记服务
        url: URL_CONFIG.TDT_CIA_C,
        layer: "tdtImg_c",
        style: "default",
        format: "tiles",
        tileMatrixSetID: "c",
        subdomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
        // eslint-disable-next-line no-undef
        tilingScheme: new Cesium.GeographicTilingScheme(),
        tileMatrixLabels: [
          "1",
          "2",
          "3",
          "4",
          "5",
          "6",
          "7",
          "8",
          "9",
          "10",
          "11",
          "12",
          "13",
          "14",
          "15",
          "16",
          "17",
          "18",
        ],
        maximumLevel: 18,
        show: false,
      })
    );

    // that.viewer.imageryLayers.addImageryProvider(new Cesium.UrlTemplateImageryProvider(
    //     {
    //         url:'http://192.168.3.121/tileServer/HFT_DOM_20180930/{z}/{x}/{y}.png',
    //         fileExtension : "png"
    //     }
    // ));

    that.viewer._cesiumWidget._creditContainer.style.display = "none"; //去掉logo
    //设置初始位置
    this.viewer.camera.setView({
      // eslint-disable-next-line no-undef
      destination: Cesium.Cartesian3.fromDegrees(
        118.63854163,
        32.04047801,
        16000000
      ),
    });

    // var tileset = new Cesium.Cesium3DTileset({
    //    url: 'http://192.168.3.121/tempServer/tileset.json'
    // });
    // this.viewer.scene.primitives.add(tileset);
    // this.viewer.zoomTo(tileset);

    // let promise = Cesium.GeoJsonDataSource.load('../../static/china.json');  //显示管线数据  直接加载json数据 比把json转化成czml在加载 快很多
    // promise.then(function (dataSource) {
    //     that.viewer.dataSources.add(dataSource);
    //     let entities = dataSource.entities.values;
    //     let colorHash = {};

    //     for (let o = 0; o < entities.length; o++) {
    //         let r = entities[o];
    //         r.nameID = o;   //给每条线添加一个编号，方便之后对线修改样式
    //         r.polyline.width = 10;  //添加默认样式
    //         (r.polyline.material = new Cesium.PolylineGlowMaterialProperty({
    //             glowPower: .1, //一个数字属性，指定发光强度，占总线宽的百分比。
    //             color: Cesium.Color.ORANGERED.withAlpha(.9)
    //         }), 10)
    //     }
    //     let temp = new Array();
    //     window.Hightlightline = function (nameid) {
    //         let exists = temp.indexOf(nameid);
    //         if (exists <= -1) {
    //             Highlight(nameid,50, 50);
    //             temp.push(nameid);  // 添加线nameID到数组，
    //         }
    //         else  //已经是高亮状态了 再次点击修改为原始状态
    //         {
    //             Highlight(nameid,10, 10);
    //             temp.splice(exists, 1);  //删除对应的nameID
    //         }
    //     }
    //     window.Highlight = function (nameid,width1, width2) {
    //         for (let o = 0; o < entities.length; o++) {
    //             let m = entities[o];
    //             if (nameid == o) {
    //                 m.polyline.width = width1;
    //                 (m.polyline.material = new Cesium.PolylineGlowMaterialProperty({
    //                     glowPower: .1, //一个数字属性，指定发光强度，占总线宽的百分比。
    //                     color: Cesium.Color.ORANGERED.withAlpha(.9)
    //                 }), width2)
    //             }
    //         }
    //     }
    // });
    // that.viewer.flyTo(promise);
    // that.viewer.screenSpaceEventHandler.setInputAction(function onLeftClick(movement) {
    //     let pickedFeature = that.viewer.scene.pick(movement.position);
    //     if (typeof (pickedFeature) != "undefined")   //鼠标是否点到线上
    //     {
    //         let name_id = pickedFeature.id.nameID;  //获取每条线的nameID
    //         Hightlightline(name_id);
    //     }
    // }, Cesium.ScreenSpaceEventType.LEFT_CLICK);
    debugger;
    var monitorEntity = that.viewer.entities.add({
      name: "video",
      // eslint-disable-next-line no-undef
      position: Cesium.Cartesian3.fromDegrees(109.44, 32.11, 30.0),
      point: {
        //点
        pixelSize: 0,
        HeightReference: 1000,
      },
      label: {
        //文字标签
        text: "AAAA",
        font: "500 30px Helvetica", // 15pt monospace
        scale: 0.5,
        // eslint-disable-next-line no-undef
        style: Cesium.LabelStyle.FILL,
        // eslint-disable-next-line no-undef
        fillColor: Cesium.Color.WHITE,
        // eslint-disable-next-line no-undef
        pixelOffset: new Cesium.Cartesian2(-8, -35), //偏移量
        showBackground: true,
        // eslint-disable-next-line no-undef
        backgroundColor: new Cesium.Color(0.5, 0.6, 1, 1.0),
      },
      billboard: {
        //图标
        image: require("./clz.png"),
        width: 25,
        height: 41,
      },
    });
    // 视角定位
    that.viewer.flyTo(monitorEntity, {
      duration: 2,
      offset: {
        // eslint-disable-next-line no-undef
        heading: Cesium.Math.toRadians(0.0),
        // eslint-disable-next-line no-undef
        pitch: Cesium.Math.toRadians(-30),
        range: 720,
      },
    });
    //点击广告牌改变文本和图标
    // eslint-disable-next-line no-undef
    var handler3D = new Cesium.ScreenSpaceEventHandler(
      that.viewer.scene.canvas
    );
    // handler3D.setInputAction(function (click) {
    //   var pick = that.viewer.scene.pick(click.position);
    //   if (pick && pick.id._name == "video") {
    //     monitorEntity._billboard._image._value = require("./ylz.png");
    //     monitorEntity._label._text._value = "label";
    //   } else {
    //     return;
    //   }
    //   // eslint-disable-next-line no-undef
    // }, Cesium.ScreenSpaceEventType.LEFT_CLICK);

    handler3D.setInputAction(function (movement) {
      var pick = that.viewer.scene.pick(movement.position);
      if (pick && pick.id) {
        console.log(pick.id);
        // eslint-disable-next-line no-undef
        $("#trackPopUp").show();
        // eslint-disable-next-line no-undef
        var cartographic = Cesium.Cartographic.fromCartesian(movement.position);
        var point = [
          (cartographic.longitude / Math.PI) * 180,
          (cartographic.latitude / Math.PI) * 180,
        ];
        // eslint-disable-next-line no-undef
        var destination = Cesium.Cartesian3.fromDegrees(
          point[0],
          point[1],
          3000.0
        );
        var id = pick.id._id.replace(/[^0-9]/gi, "");
        var content =
          '<table><tbody><tr><th style="color:black;">' +
          pick.id._name +
          '</th><td><button style="color:black;" οnclick="updateValve(' +
          id +
          ')">确定</button></td><td><button οnclick="deleteValve(' +
          id +
          ')" style="color:black;">删除</button></td></tr>' +
          '<tr><th style="color:black;">类型</th><td><input style="color:black;" value=111></td></tr>' +
          '<tr><th style="color:black;">经度</th><td><input id="x" style="color:black;" value=222></td></tr>' +
          '<tr><th style="color:black;">纬度</th><td><input id="y" style="color:black;" value=333></td></tr>' +
          "</tbody></table>";
        var obj = {
          position: movement.position,
          destination: destination,
          content: content,
        };
        that.infoWindow(obj);
        // eslint-disable-next-line no-undef
        // function infoWindow(obj) {
        //   var picked = that.viewer.scene.pick(obj.position);
        //   // eslint-disable-next-line no-undef
        //   if (Cesium.defined(picked)) {
        //     // eslint-disable-next-line no-undef
        //     var id = Cesium.defaultValue(picked.id, picked.primitive.id);
        //     // eslint-disable-next-line no-undef
        //     if (id instanceof Cesium.Entity) {
        //       // eslint-disable-next-line no-undef
        //       $(".cesium-selection-wrapper").show();
        //       $("#trackPopUpLink").empty();
        //       $("#trackPopUpLink").append(obj.content);
        //       // eslint-disable-next-line no-inner-declarations
        //       function positionPopUp(c) {
        //         var x = c.x - $("#trackPopUpContent").width() / 2;
        //         var y = c.y - $("#trackPopUpContent").height();
        //         $("#trackPopUpContent").css(
        //           "transform",
        //           "translate3d(" + x + "px, " + y + "px, 0)"
        //         );
        //       }
        //       // eslint-disable-next-line no-undef
        //       var c = new Cesium.Cartesian2(obj.position.x, obj.position.y);
        //       $("#trackPopUp").show();
        //       positionPopUp(c); // Initial position
        //       // at the place item
        //       // picked
        //       removeHandler = viewer.scene.postRender.addEventListener(
        //         function () {
        //           if (picked.id._polyline != null) {
        //             var pos = {};
        //             pos.x =
        //               (id._polyline._positions._value["0"].x +
        //                 id._polyline._positions._value[1].x) /
        //               2;
        //             pos.y =
        //               (id._polyline._positions._value["0"].y +
        //                 id._polyline._positions._value[1].y) /
        //               2;
        //             pos.z =
        //               (id._polyline._positions._value["0"].z +
        //                 id._polyline._positions._value[1].z) /
        //               2;
        //             var changedC = Cesium.SceneTransforms.wgs84ToWindowCoordinates(
        //               viewer.scene,
        //               pos
        //             );
        //           } else {
        //             var changedC = Cesium.SceneTransforms.wgs84ToWindowCoordinates(
        //               viewer.scene,
        //               id._position._value
        //             );
        //           } // If things moved, move the
        //           // popUp too
        //           if (c.x !== changedC.x || c.y !== changedC.y) {
        //             positionPopUp(changedC);
        //             c = changedC;
        //           }
        //         }
        //       );
        //       // PopUp close button event handler
        //       $(".leaflet-popup-close-button").click(function () {
        //         $("#trackPopUp").hide();
        //         $("#trackPopUpLink").empty();
        //         $(".cesium-selection-wrapper").hide();
        //         removeHandler.call();
        //         return false;
        //       });
        //       return id;
        //     }
        //   }
        // }
      } else {
        // eslint-disable-next-line no-undef
        $("#trackPopUp").hide();
      }
      // eslint-disable-next-line no-undef
    }, Cesium.ScreenSpaceEventType.LEFT_CLICK);
  },
  data() {
    return {
      viewer: {},
    };
  },
  methods:{
    infoWindow(obj) {
      let that = this
      var picked = that.viewer.scene.pick(obj.position);
      // eslint-disable-next-line no-undef
      if (Cesium.defined(picked)) {
        // eslint-disable-next-line no-undef
        var id = Cesium.defaultValue(picked.id, picked.primitive.id);
        // eslint-disable-next-line no-undef
        if (id instanceof Cesium.Entity) {
          // eslint-disable-next-line no-undef
          $(".cesium-selection-wrapper").show();
          // eslint-disable-next-line no-undef
          $("#trackPopUpLink").empty();
          // eslint-disable-next-line no-undef
          $("#trackPopUpLink").append(obj.content);
          // eslint-disable-next-line no-inner-declarations
          function positionPopUp(c) {
            // eslint-disable-next-line no-undef
            var x = c.x - $("#trackPopUpContent").width() / 2;
            // eslint-disable-next-line no-undef
            var y = c.y - $("#trackPopUpContent").height();
            // eslint-disable-next-line no-undef
            $("#trackPopUpContent").css(
              "transform",
              "translate3d(" + x + "px, " + y + "px, 0)"
            );
          }
          // eslint-disable-next-line no-undef
          var c = new Cesium.Cartesian2(obj.position.x, obj.position.y);
          // eslint-disable-next-line no-undef
          $("#trackPopUp").show();
          positionPopUp(c); // Initial position
          // at the place item
          // picked
          let changedC = null;
          let removeHandler = that.viewer.scene.postRender.addEventListener(
            function () {
              if (picked.id._polyline != null) {
                var pos = {};
                pos.x =
                  (id._polyline._positions._value["0"].x +
                    id._polyline._positions._value[1].x) /
                  2;
                pos.y =
                  (id._polyline._positions._value["0"].y +
                    id._polyline._positions._value[1].y) /
                  2;
                pos.z =
                  (id._polyline._positions._value["0"].z +
                    id._polyline._positions._value[1].z) /
                  2;
                // eslint-disable-next-line no-undef
                changedC = Cesium.SceneTransforms.wgs84ToWindowCoordinates(
                  that.viewer.scene,
                  pos
                );
              } else {
                // eslint-disable-next-line no-undef
                changedC = Cesium.SceneTransforms.wgs84ToWindowCoordinates(
                  that.viewer.scene,
                  id._position._value
                );
              } // If things moved, move the
              // popUp too
              if (c.x !== changedC.x || c.y !== changedC.y) {
                positionPopUp(changedC);
                c = changedC;
              }
            }
          );
          // PopUp close button event handler
          // eslint-disable-next-line no-undef
          $(".leaflet-popup-close-button").click(function () {
            // eslint-disable-next-line no-undef
            $("#trackPopUp").hide();
            // eslint-disable-next-line no-undef
            $("#trackPopUpLink").empty();
            // eslint-disable-next-line no-undef
            $(".cesium-selection-wrapper").hide();
            removeHandler.call();
            return false;
          });
          return id;
        }
      }
    }
  }
};
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#cesiumContainer {
  position: absolute;
  width: 100%;
  height: 100%;
}

/*leaflet风格气泡窗口样式模板*/
.leaflet-popup {
  position: absolute;
  text-align: center;
}
.leaflet-popup-close-button {
  position: absolute;
  top: 0;
  right: 0;
  padding: 4px 4px 0 0;
  text-align: center;
  width: 18px;
  height: 14px;
  font: 16px/14px Tahoma, Verdana, sans-serif;
  color: #c3c3c3;
  text-decoration: none;
  font-weight: bold;
  background: transparent;
}
.leaflet-popup-content-wrapper {
  text-align: center;
  max-height: 200px;
  overflow-y: auto;
  background: white;
  box-shadow: 0 3px 14px rgba(0, 0, 0, 0.4);
  padding: 1px;
  text-align: left;
  border-radius: 12px;
}
.leaflet-popup-content {
  margin: 13px 19px;
  line-height: 1.4;
}
.leaflet-popup-tip-container {
  margin: 0 auto;
  width: 200px;
  height: 40px;
  position: relative;
  overflow: hidden;
}
.leaflet-popup-tip {
  background: white;
  box-shadow: 0 3px 14px rgba(0, 0, 0, 0.4);
  width: 17px;
  height: 17px;
  padding: 1px;
  margin: -10px auto 0;
  -webkit-transform: rotate(45deg);
  -moz-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  -o-transform: rotate(45deg);
  transform: rotate(45deg);
}

/*按钮样式*/
.add-button {
}
</style>
