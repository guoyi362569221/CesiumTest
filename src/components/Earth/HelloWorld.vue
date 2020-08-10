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

    that.drawWarningListPoints(that.disasterPoints);
  },
  data () {
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
    }
  },
  methods:{
    /*绘制功能*/
    drawWarningListPoints(pointsGroup) {
      let that = this;
        for (let i = 0; i < pointsGroup.length; i++) {
            let longtitude = parseFloat(pointsGroup[i].JCDINFO01A110);
            let lattitude =parseFloat(pointsGroup[i].JCDINFO01A120);
            let height = 500/*高度*/;
            let color = pointsGroup[i].YJLEVEL === 3 ? 'yellow' : (pointsGroup[i].YJLEVEL === 2 ? 'orange' : 'red');
            if (longtitude && lattitude) {
              debugger
                let iconPath = require("./assets/img/"+color+".gif");
                let curId = 'gifImg_' + i;
                
                // eslint-disable-next-line no-undef
                let position = Cesium.Cartesian3.fromDegrees(longtitude, lattitude, height);
                let html = `<img class="gifImg" id="${curId}" src="${iconPath}" alt="" style="position: absolute;"/>`;
                // eslint-disable-next-line no-undef
                new Cesium.DivPoint(that.viewer, {
                    html: html,
                    position: position,
                    anchor: [0, 0],
                    noEvent: false
                });
                // $('#' + curId).on('click',
                //   function () {
                //     let data = pointsGroup[i].datas;
                //     let position = Cesium.Cartesian3.fromDegrees(longtitude, lattitude, height);
                //     // 预警等级低=》高3-2-1，黄-橙-红
                //     let colorStr = data.YJLEVEL === 3 ? '黄色' : (data.YJLEVEL === 2 ? '橙色' : '红色');
                //     let levelStr = data.YJLEVEL === 3 ? 'Ⅲ级' : (data.YJLEVEL === 2 ? 'Ⅱ级' : 'Ⅰ级');
                //     let levelTxt = data.YJLEVEL === 3 ? '中型地质灾害黄色预警' : (data.YJLEVEL === 2 ? '大型地质灾害橙色预警' : '特大型地质灾害红色预警');
                //     let levelText = data.YJLEVEL === 3 ? '24小时内发生可能性很大<br/><span style="margin-left: 63px;">(需通知监测人员和威胁住户注意)</span>' : (data.YJLEVEL === 2 ? '24小时内发生可能性很大<br/><span style="margin-left: 63px;">(预报阶段，停止外业，各岗位人员到岗待命)</span>' : '24小时内发生可能性很大<br/><span style="margin-left: 63px;">(报警阶段，无条件紧急疏散，密切观测)</span>');
                //     let colorClass = data.YJLEVEL === 3 ? 'yellow' : (data.YJLEVEL === 2 ? 'orange' : 'red');
                //     data.CJMK = (data.CJMK == undefined ? '' : data.CJMK).replace(/_/g,"");
                //     let content = `
                //                   <div id=${'layerBoxContainer_' + curId} class="layerBoxContainer">
                //                       <div class="layerBox pr layerBox_yjxx" style="background: white;width: 320px;height: 300px">
                //                           <div class="tabTitle"><span style="padding: 5px">${data.CJMK == undefined ? '未命名' : data.CJMK}${colorStr}预警</span><i class="tabName"></i><span class="closeX" onclick="closeTab(${'layerBoxContainer_' + curId})"></span></div>
                //                               <dl>
                //                                   <dd><span>灾害名称： </span><span id="zhmc">${data.JCAA02A020 == undefined ? '' : data.JCAA02A020}</span></dd>
                //                                   <dd><span>设备名称： </span><span id="sbmc">${data.CJMK == undefined ? '' : data.CJMK}</span></dd>
                //                                   <dd><span>预警时间： </span><span id="yjsj">${data.JCTIME == undefined ? '' : COMMON.formatDateTime(data.JCTIME)}</span></dd>
                //                                   <dd><span>监测数值： </span><span id="jcz">${data.YJVALUE == undefined ? '' : data.YJVALUE}</span></dd>
                //                                   <dd><span>预警级别： </span> <i class="${colorClass}Txt" id="yjjb">${levelStr}</i></dd>
                //                                   <dd><span>预警显示： </span><span>${levelTxt}</span></dd>
                //                                   <dd><span>发生概率： </span><span>${levelText}</span></dd>
                //                               </dl>
                //                               <div class="txtR mt10 pa btnGroup">
                //                                   <button type="button" class="layui-btn layui-btn-sm layui-btn-normal" data-method="yjcl" >预警策略</button>
                //                               </div>
                //                           </div>
                //                       <div class="selfTriangle"></div>
                //                   </div>
                //     `;
                //     if (document.getElementById('layerBoxContainer_' + curId))
                //     {document.getElementById('layerBoxContainer_' + curId).remove();}
                //     let weatherDiv = new Cesium.DivPoint(viewer3D, {
                //         html: content,
                //         position: position,
                //         anchor: [-160, -312],
                //         noEvent: false
                //     });
                // })
            } else {
                return false;
            }
        }
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
