<template>
  <div id="cesiumContainer">
    <div id="trackPopUp" class="noselect" style="display:none;">
      <div id="trackPopUpContent" class="leaflet-popup" style="top:5px;left:0;z-index:1;">
        <a class="leaflet-popup-close-button" href="#">×</a>
        <div class="leaflet-popup-content-wrapper">
          <div id="trackPopUpLink" class="leaflet-popup-content" style="max-width: 350px;"></div>
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
    let monitorEntity = null
    for(let i=0;i<20;i++){
      let itemObj = {
        name: "video",
        // eslint-disable-next-line no-undef
        position: Cesium.Cartesian3.fromDegrees(109.44+i, 32.11+i, 0),
        point: {
          //点
          pixelSize: 0,
          // heightReference: 0,
          // disableDepthTestDistance: Number.POSITIVE_INFINITY,
          // eslint-disable-next-line no-undef
          // heightReference: Cesium.HeightReference.CLAMP_TO_GROUND,//设置HeightReference高度参考类型为CLAMP_TO_GROUND贴地类型
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
          pixelOffset: new Cesium.Cartesian2(-0, -35), //偏移量
          showBackground: true,
          // eslint-disable-next-line no-undef
          backgroundColor: new Cesium.Color(0.5, 0.6, 1, 1.0),
          disableDepthTestDistance: Number.POSITIVE_INFINITY,
          // eslint-disable-next-line no-undef
          // heightReference:Cesium.HeightReference.CLAMP_TO_GROUND
        },
        billboard: {
          //图标
          image: require("./clz.png"),
          // // eslint-disable-next-line no-undef
          // image:new Cesium.GifImageProperty({
          //     url: require("./orange.gif")
          // }),
          width: 25,
          height: 41,
          // eslint-disable-next-line no-undef
          disableDepthTestDistance: Number.POSITIVE_INFINITY,
          // eslint-disable-next-line no-undef
          // heightReference:Cesium.HeightReference.CLAMP_TO_GROUND
        },
      }
      monitorEntity = that.viewer.entities.add(itemObj);
    }

    
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
    let handler3D = new Cesium.ScreenSpaceEventHandler(
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

    // ScreenSpaceEventHandler   处理用户输入事件。可以添加自定义功能以在以下位置执行当用户输入输入时。
    handler3D.setInputAction(function(movement) {
        // 监听鼠标的当前位置坐标，然后根据当前坐标去动态更新气泡窗口div的显示位置；
        let pick = that.viewer.scene.pick(movement.position);
        // ovement.position是获取屏幕坐标
        // pick 记录当前屏幕位置
        if(pick && pick.id._name == "video"){
            // 这里的判断条件还是蛮有用的,比如你点击某些点的时候想弹出自定义弹窗,其他点弹出原生弹窗,就需要在这里进行判断了
            // eslint-disable-next-line no-undef
            $("#trackPopUp").show();
            // 显示弹窗容器
            // eslint-disable-next-line no-undef
            let cartographic = Cesium.Cartographic.fromCartesian(pick.primitive._actualPosition);
            // 获取点的经纬度
            let point=[cartographic.longitude / Math.PI * 180, cartographic.latitude / Math.PI * 180];
            // 转换坐标
            // eslint-disable-next-line no-undef
            let destination=Cesium.Cartesian3.fromDegrees(point[0], point[1], 3000.0);
            // destination是我们点击之后,flyto的位置
            let content =
                '<table  border="1" width="400px" colspan="4">'+
                '    <thead ><th colspan="4" style="text-align: center">七里河区第二号地质灾害点</th></thead>'+
                '    <tbody>'+
                '    <tr >'+
                '        <th align="left">地理位置:甘肃省兰州市起立河区<br/>管道桩号:7578323N<br/>灾害类型:小型<br/>风险等级:严重</th>'+
                '        <th align="center"><img src="' + '凯尔特人.jpg' + ' " alt="灾害点图片加载失败" width="100px" height="100px"></th>'+
                '    </tr>'+
                '    </tbody>'+
                '    <tfoot >'+
                '    <td colspan="4" align="center">'+
                '        <button style="color: #003da8">基本特征</button>'+
                '        <button style="color: #003da8" οnclick="this">勘察设计</button>'+
                '        <button style="color: #003da8">巡检卡</button>'+
                '        <button style="color: #003da8">历年对比</button>'+
                '        <button style="color: #003da8">巡查上报</button></td>'+
                '    </tfoot>'+
                '</table>';
            // content是核心,你想弹出的东西,就全部放在这里面
            var obj = {position:movement.position,destination:destination,content:content};
            // 构造一个参数,包括事件、 位置、已经弹框
            that.infoWindow(obj,pick);
        }
        else{
          // eslint-disable-next-line no-undef
          $('#trackPopUp').hide();
        }
        // eslint-disable-next-line no-undef
    }, Cesium.ScreenSpaceEventType.LEFT_CLICK);
  },
  data() {
    return {
      viewer: {},
      disasterPoints:[
            {
                "JCAA02A350": 1515024000000,
                "JCDINFO01A030": "3",
                "DELETEFLAG": "0",
                "JCAA02A390": 2,
                "JCDINFO01A110": 110.616236,
                "JCAA02A070": 110.6156,
                "JCTIME": 1588079580000,
                "MINVALUE_TIME": 1540512000000,
                "CLMC": "树坪滑坡雨量黄色预警",
                "JCAA02A200": 1578048841000,
                "JCAA02A240": "9824677865a740c3bb03c8ff671125a9",
                "JCDINFO01A040": "42052700000001000003",
                "MAXVALUE_TIME": 1588032000000,
                "JCDINFO01A120": 30.992684,
                "JCAA02A160": 420527104009,
                "JC_MINVALUE": 89.23,
                "JCAA02A040": "01",
                "OID": "2",
                "TYBH": "05162300008",
                "JCAA02A080": 30.99139,
                "JCDSB01A050": 0,
                "YJVALUE": 60.32,
                "JC_MAXVALUE": 121.12,
                "JCDSB01A010": "ace675a5741511eaae810011322ca603",
                "JCAA02A410": "13",
                "JCDINFO01A050": "YL01",
                "JCAA02A010": "42052700000001000003",
                "JCAA02A330": "LTS",
                "JCDINFO01A130": "滑坡中后部",
                "JCDINFO01A010": "3573f6ce892611eaae810011322ca603",
                "JCAA02A170": "9824677865a740c3bb03c8ff671125a9",
                "JCAA02A370": 0,
                "SFWSJD": 2,
                "READSTATE": 0,
                "JCAA02A290": "18071318052",
                "YJLEVEL": 2,
                "CJMK": "树坪滑坡_雨量监测",
                "JCDSB01A020": "420527000000010000030024",
                "JCAA02A420": "13972002480,18911630214,13007191908,15565749532",
                "JCAA02A300": "1baf8e36-bc2e-41fa-bb42-d0e41d90eded",
                "JCDINFO01A060": 1564963200000,
                "JCAA02A020": "树坪滑坡监测网",
                "JCDINFO01A140": "11",
                "JCAA02A220": "树坪滑坡体在监测初期布设6个GPS变形监测点，滑体外围布设1个GPS监测基点。变形监测点呈两纵三横布置，基本能监控整个滑坡体的变形。2004年2月，因滑坡体变形加剧，根据滑坡体变形特点，在滑体前缘及中部临时增设5个GPS变形监测点投入应急监测，2006年5月监测点SP-5已完全被毁，2006年12月监测点SP-1、SP-3已被库水淹没。2014年11月，ZG89点因防治工程施工被破坏。2007年10月24日在树坪滑坡体上新建一个GPS监测点SP-6及基准点spj1，2007年9月26日又新建了一个基准点sp-2，这两个基准点是为应急监测用。2008年7月监测点SP-4因移土培肥破坏而失测。2010年4月在树坪滑坡体上新增4个应急倾斜监测钻孔、1个水文监测钻孔、1个推力监测钻孔，其中倾斜监测孔QZK4于2010年7月损坏，QZK1、QZK2及水文孔、推力孔目前均不能正常工作，同时，在各GPS监测点上高通公司实施了实时相对位移监测。\n2019年，为了与以往专业监测设备进行对比验证普适型监测设备的功能成效，将该点选择为湖北省地质灾害三级监测试点，同时该点也作为国家普适型监测试点，在树坪滑坡体上新增各类普适型监测设备，包括：GNSS、加速度计及综合监测设备等不同类型的监测设备。",
                "JCDINFO01A020": "420527000000010000030024",
                "JCAA02A060": "湖北省宜昌市秭归县沙镇溪镇树坪村",
                "JCDINFO01A100": "中科川信布设雨量计",
                "JCAA02A260": "1",
                "JCAA02A140": 1340150400000,
                "JCDSB01A030": "05162300008",
                "JGSJ": 3
            }, {
                "JCAA02A310": "黄照先",
                "JCAA02A350": 1569888000000,
                "JCDINFO01A030": "1",
                "DELETEFLAG": "0",
                "JCAA02A390": 3,
                "JCDINFO01A110": 110.743337,
                "JCAA02A070": 110.7439,
                "JCTIME": 1588077125000,
                "MINVALUE_TIME": 1588077148000,
                "CLMC": "树坪滑坡GNSS黄色预警",
                "JCAA02A200": 1576851052000,
                "JCAA02A240": "295a73ea381e441281fe4c9f1eb04f63",
                "JCDINFO01A040": "42052710600701000000",
                "MAXVALUE_TIME": 1588077145000,
                "JCAA02A320": "18986766680",
                "JCDINFO01A120": 30.917907,
                "JCAA02A280": "张瑞淼",
                "JCAA02A160": 420527106007,
                "JC_MINVALUE": 45.32,
                "JCAA02A040": "01",
                "OID": "3",
                "TYBH": "05162300007",
                "JCAA02A080": 30.91972,
                "JCDSB01A050": 0,
                "YJVALUE": 32.23,
                "JC_MAXVALUE": 21.12,
                "JCDSB01A010": "ace635fc741511eaae810011322ca603",
                "JCAA02A410": "13",
                "JCDINFO01A050": "XZF9242",
                "JCAA02A010": "42052710600701000000",
                "JCAA02A330": "LTS",
                "JCDINFO01A130": "滑坡中部",
                "JCDINFO01A010": "3569c6f0892611eaae810011322ca603",
                "JCAA02A170": "9824677865a740c3bb03c8ff671125a9",
                "JCAA02A370": 0,
                "SFWSJD": 1,
                "READSTATE": 0,
                "YJLEVEL": 1,
                "CJMK": "树坪滑坡_GNSS监测",
                "JCDSB01A020": "420527106007010000000018",
                "JCAA02A420": "13972002480,18911630214,13007191908,15565749532",
                "JCAA02A300": "1baf8e36-bc2e-41fa-bb42-d0e41d90eded",
                "JCDINFO01A060": 1563840000000,
                "JCAA02A020": "小榨坊下游崩滑体三级监测网",
                "JCDINFO01A140": "08",
                "JCAA02A220": "该灾害点为湖北省三级地质灾害专业监测试点，同时也作为国家普适性监测设备试点。在滑坡上布有多种监测设备，包括：GNSS、裂缝计、倾角计、加速度计及综合监测设备等不同类型的监测设备，在监测滑坡表层形变特征同时也进行互相对比验证。",
                "JCDINFO01A020": "420527106007010000000018",
                "JCAA02A060": "湖北省宜昌市秭归县郭家坝镇烟灯堡村",
                "JCDINFO01A100": "东方生态综合监测设备（土壤含水率、土壤温度、倾斜角）",
                "JCAA02A260": "1",
                "JCAA02A140": 1569801600000,
                "JCAA02A180": "刘哲儒",
                "JCDSB01A030": "05162300007",
                "JGSJ": 3
            }]
    };
  },
  methods:{
    infoWindow(obj,pick) {
      let that = this
        let picked = that.viewer.scene.pick(obj.position);
        // 首先获取点击点的信息
        // eslint-disable-next-line no-undef
        if (Cesium.defined(picked)) {
            // 判断 如果点被定义了
            // eslint-disable-next-line no-undef
            let id = Cesium.defaultValue(picked.id, picked.primitive.id); 
            // 获取id(id就是原生弹窗的标题)
            if(id ){
                let back_position = null
                if (obj.destination) {
                  // eslint-disable-next-line no-undef
                    back_position = new Cesium.Cartesian3.fromDegrees(that.getCenterPosition().x, that.getCenterPosition().y, that.getCenterPosition().z);
                    // 我在这里用back_position记录的点击之前的位置,便于×掉弹窗后返回
                    // that.viewer.camera.flyTo({
                    //     // 跳转到我们刚才定义的位置
                    //     destination: obj.destination
                    // });
                }
                // 填充内容
                // eslint-disable-next-line no-undef
                $(".cesium-selection-wrapper").show();
                // cesium-selection-wrapper是cesium内置的东西
                // eslint-disable-next-line no-undef
                $('#trackPopUpLink').empty();   
                // empty() 方法从被选元素移除所有内容，包括所有文本和子节点。
                // eslint-disable-next-line no-undef
                $('#trackPopUpLink').append(obj.content);  
                // append() 方法在被选元素的结尾（仍然在内部）插入指定内容。
                // eslint-disable-next-line no-undef
                var c = new Cesium.Cartesian2(obj.position.x, obj.position.y);
                // eslint-disable-next-line no-undef
                $('#trackPopUp').show();
                that.positionPopUp(c); 
                //实时更新位置
                let removeHandler = that.viewer.scene.postRender.addEventListener(function () {
                  // eslint-disable-next-line no-undef
                    let changedC = Cesium.SceneTransforms.wgs84ToWindowCoordinates(that.viewer.scene, pick.primitive._actualPosition);
                    // 我们转动地球,也会实时更新弹窗的位置.并不会一成不变
                    if (c && changedC && c.x && changedC.x && c.y && changedC.y) {
                        if ((c.x !== changedC.x) || (c.y !== changedC.y)) {
                            that.positionPopUp(changedC);
                            c = changedC;
                        }
                    }
                });
                // PopUp close button event handler
                // eslint-disable-next-line no-undef
                $('.leaflet-popup-close-button').click(function () {
                  // eslint-disable-next-line no-undef
                    $('#trackPopUp').hide();
                    // eslint-disable-next-line no-undef
                    $('#trackPopUpLink').empty();
                    // eslint-disable-next-line no-undef
                    $(".cesium-selection-wrapper").hide();
                    if(removeHandler){
                      removeHandler.call();
                    }
                    if(back_position){
                      // that.viewer.camera.flyTo({
                      //   // 回到我们点击前的位置
                      //     destination: back_position
                      // });
                    }
                    
                    return false;
                });

            }
        }
    },
    getHeight(){
      let that = this
      //获取当前高度
      if (that.viewer) {
          let scene = that.viewer.scene;
          let ellipsoid = scene.globe.ellipsoid;
          let height = ellipsoid.cartesianToCartographic(that.viewer.camera.position).height;
          return height;
      }
    },
    getCenterPosition(){
      let that = this
      //获取当前位置
      // eslint-disable-next-line no-undef
      let result = that.viewer.camera.pickEllipsoid(new Cesium.Cartesian2(that.viewer.canvas.clientWidth / 2, that.viewer.canvas
          .clientHeight / 2));
          // eslint-disable-next-line no-undef
      let curPosition = Cesium.Ellipsoid.WGS84.cartesianToCartographic(result);
      let lon = curPosition.longitude * 180 / Math.PI;
      let lat = curPosition.latitude * 180 / Math.PI;
      let height = that.getHeight();
      return {
          x: lon,
          y: lat,
          z: height
      };
    },
    positionPopUp(c){
      // eslint-disable-next-line no-undef
      let x = c.x - ($('#trackPopUpContent').width()) / 2;
      // eslint-disable-next-line no-undef
      let y = c.y - ($('#trackPopUpContent').height());
      // 为所有匹配元素(#trackPopUpContent)设置transform的值为 'translate3d(' + x + 'px, ' + y + 'px, 0)'
      // eslint-disable-next-line no-undef
      $('#trackPopUpContent').css('transform', 'translate3d(' + x + 'px, ' + y + 'px, 0)');
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
  overflow: hidden;
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
    box-shadow: 0 3px 14px rgba(0,0,0,0.4);
    padding: 1px;
    text-align: left;
    border-radius: 12px;
}
.leaflet-popup-content {
    margin: 13px 19px;
    line-height: 1.4;
}
.leaflet-popup-tip-container {
    /*修改弹窗位置*/
    margin: 0 auto;
    width: 200px;
    height: 50px;
    position: relative;
    overflow: hidden;
}
.leaflet-popup-tip {
    background: white;
    box-shadow: 0 3px 14px rgba(0,0,0,0.4);
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
</style>
