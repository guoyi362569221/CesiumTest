<template>
	<div id="cesiumContainer">
	</div>
</template>

<script>
export default {
  name: 'cesiumContainer',
  mounted () {

    let that = this;

    //天地图URL配置
    let URL_CONFIG = {
      TDT_IMG_W: "http://{s}.tianditu.gov.cn/img_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=img&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7"   //在线天地图影像服务地址(墨卡托投影)
      , TDT_VEC_W: "http://{s}.tianditu.gov.cn/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=vec&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7"   //在线天地图矢量地图服务(墨卡托投影)
      , TDT_CIA_W: "http://{s}.tianditu.gov.cn/cia_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=cia&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default.jpg&tk=ca9c419b9135b2179d0ff37413bba8b7"            //在线天地图影像中文标记服务(墨卡托投影)
      , TDT_CVA_W: "http://{s}.tianditu.gov.cn/cva_w/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=cva&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default.jpg&tk=ca9c419b9135b2179d0ff37413bba8b7"            //在线天地图矢量中文标记服务(墨卡托投影)
      , TDT_IMG_C: "http://{s}.tianditu.gov.cn/img_c/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=img&tileMatrixSet=c&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7"  //在线天地图影像服务地址(经纬度)
      , TDT_VEC_C: "http://{s}.tianditu.gov.cn/vec_c/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=vec&tileMatrixSet=c&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7"   //在线天地图矢量地图服务(经纬度)
      , TDT_CIA_C: "http://{s}.tianditu.gov.cn/cia_c/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=cia&tileMatrixSet=c&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7"      //在线天地图影像中文标记服务(经纬度)
      , TDT_CVA_C: "http://{s}.tianditu.gov.cn/cva_c/wmts?service=wmts&request=GetTile&version=1.0.0" +
        "&LAYER=cva&tileMatrixSet=c&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}" +
        "&style=default&format=tiles&tk=ca9c419b9135b2179d0ff37413bba8b7"       //在线天地图矢量中文标记服务(经纬度)
    };

    //buildModuleUrl.setBaseUrl('../../static/Cesium/')
    debugger
    // eslint-disable-next-line no-undef
    that.viewer = new Cesium.Viewer('cesiumContainer', {
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
        tileMatrixLabels: ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18"],
        maximumLevel: 18,
        show: false
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
    that.viewer.imageryLayers.addImageryProvider(new Cesium.WebMapTileServiceImageryProvider({   //调用影响中文注记服务
      url: URL_CONFIG.TDT_CIA_C,
      layer: "tdtImg_c",
      style: "default",
      format: "tiles",
      tileMatrixSetID: "c",
      subdomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
      // eslint-disable-next-line no-undef
      tilingScheme: new Cesium.GeographicTilingScheme(),
      tileMatrixLabels: ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12", "13", "14", "15", "16", "17", "18"],
      maximumLevel: 18,
      show: false
    }));

    // that.viewer.imageryLayers.addImageryProvider(new Cesium.UrlTemplateImageryProvider(
    //     {
    //         url:'http://192.168.3.121/tileServer/HFT_DOM_20180930/{z}/{x}/{y}.png',
    //         fileExtension : "png"
    //     }
    // ));

    that.viewer._cesiumWidget._creditContainer.style.display = "none";  //去掉logo
    //设置初始位置
    this.viewer.camera.setView({
      // eslint-disable-next-line no-undef
      destination: Cesium.Cartesian3.fromDegrees(118.63854163, 32.04047801, 16000000)
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
  },
  data () {
    return {
      viewer: {}
    }
  }
}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#cesiumContainer{
  position: absolute;
  width: 100%;
  height: 100%;
}
</style>
