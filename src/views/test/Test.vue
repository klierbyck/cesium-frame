<template>
    <div>
        <div id="cesiumBox"></div>
        <div id="credit"></div>
    </div>
</template>
<script>
    export default {
        data () {
            return {

            }
        },
        mounted () {
            let viewer = new this.Cesium.Viewer('cesiumBox', {
                geocoder: false,
                homeButton: false,
                sceneModePicker: false,
                baseLayerPicker: false,
                navigationHelpButton: false,
                animation: false,
                creditContainer: "credit",
                timeline: false,
                fullscreenButton: false,
                vrButton: false,
                imageryProvider: new this.Cesium.WebMapTileServiceImageryProvider({
                    url: "http://t0.tianditu.com/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=vec&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default&format=tiles&tk=a7210ed6e9196896338564cb37289650",
                    layer: "tdtVecBasicLayer",
                    style: "default",
                    format: "image/jpeg",
                    tileMatrixSetID: "GoogleMapsCompatible",
                    show: false
                })
            });
            viewer.imageryLayers.addImageryProvider(new this.Cesium.WebMapTileServiceImageryProvider({
                url: "http://t0.tianditu.com/cva_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=cva&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default.jpg&tk=a7210ed6e9196896338564cb37289650",
                layer: "tdtAnnoLayer",
                style: "default",
                format: "image/jpeg",
                tileMatrixSetID: "GoogleMapsCompatible",
                show: false
            }));
            // let dataDetail;
            // this.$http.get('api/map/business/getNwfData?&tableId=41&type=1&lat1=22.906494140625&lat2=30.904541015625&lng1=97.679443359375&lng2=115.718994140625&searchType=3').then((result) => {
            //     dataDetail = result;
            // })
            //柱状
            var czml = [
                {
                    "id": "document", "name": "box", "version": "1.0",
                    "clock": {
                        "interval": "2019-01-01T00:00:00Z/2019-01-01T00:00:05Z",
                        "currentTime": "2019-01-01T00:00:00Z",
                        "multiplier": 1,
                        "range": "CLAMPED",
                        "step": "SYSTEM_CLOCK_MULTIPLIER"
                    }

                },
                {
                    "id": 'eee', "name": 'box1',
                    "position": {
                        "cartographicDegrees": [106.63, 26.65, 30000],
                    },
                    "cylinder": {
                        length: 30000,
                        topRadius: 1000,
                        bottomRadius: 1000,
                        "material": { "solidColor": { "color": 'purple' } },
                    }
                }
            ];
            const arrRgba = [
                [1, 152, 189, 255],
                [73, 227, 206, 255],
                [216, 254, 181, 255],
                [254, 237, 177, 255],
                [254, 173, 84, 255],
                [209, 55, 78, 255],
            ]
            let i = 0;
            // dataDetail.data.map(data => {
            //     let t = data.total_cnt / 300;
            //     let tC = t;
            //     if (tC > 1) tC = 1;
            //     let agba = arrRgba[parseInt(tC * 5)];
            //     let length = parseInt(t * 30) * 1000.0;
            //     let czmlObj = {
            //         "id": data.dataid, "name": data.dataname,
            //         "position": {
            //             "cartographicDegrees": [data.geolng, data.geolat, length / 2],
            //         },
            //         "cylinder": {
            //             length,
            //             topRadius: 1000,
            //             bottomRadius: 1000,
            //             "material": { "solidColor": { "color": { "rgba": agba } } },
            //         }
            //     }
            //     return czmlObj;
            // }).forEach(data => czml.push(data));

            //视角
            viewer.scene.globe.enableLighting = true;
            var initialPosition = this.Cesium.Cartesian3.fromDegrees(109.61181640625001, 23.252197265625, 453000);
            var initialOrientation = new this.Cesium.HeadingPitchRoll.fromDegrees(-35, -45, 0.0716951918898415);
            viewer.scene.camera.setView({
                destination: initialPosition,
                orientation: initialOrientation,
                endTransform: this.Cesium.Matrix4.IDENTITY
            });
            setTimeout(() => {
                var dataSourcePromise = this.Cesium.CzmlDataSource.load(czml);
                viewer.dataSources.add(dataSourcePromise);
            }, 3000)
        },
        methods: {
            
        },
    };
</script>
<style lang="less">
    @import url("./test.less");
</style>
import React, { Component, Fragment } from 'react';
import DocumentTitle from 'react-document-title';
import { connect } from 'dva';
import $ from 'jquery';
import buildModuleUrl  from 'cesium/Source/Core/buildModuleUrl';
buildModuleUrl.setBaseUrl('./cesium/');

// import Viewer from "cesium/Source/Widgets/Viewer/Viewer";
// import WebMapTileServiceImageryProvider from "cesium/Source/Scene/WebMapTileServiceImageryProvider";
// import 'cesium/Source/Widgets/widgets.css';

@connect(({ user }) => ({
    currentUser: user.currentUser,
  }))
class Index extends Component {
  constructor(props) {
    super(props);
  }

  componentDidMount(){
    const viewer = new Cesium.Viewer(this.cesiumContainer,{
      geocoder:false,
      homeButton:false,
      sceneModePicker:false,
      baseLayerPicker:false,
      navigationHelpButton:false,
      animation:false,
      creditContainer:"credit",
      timeline:false,
      fullscreenButton:false,
      vrButton:false,
      imageryProvider : new Cesium.WebMapTileServiceImageryProvider({
        url: "http://t0.tianditu.com/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=vec&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default&format=tiles&tk=a7210ed6e9196896338564cb37289650",
        layer: "tdtVecBasicLayer",
        style: "default",
        format: "image/jpeg",
        tileMatrixSetID: "GoogleMapsCompatible",
        show: false
      })
    });
    
    viewer.imageryLayers.addImageryProvider(new Cesium.WebMapTileServiceImageryProvider({
      url: "http://t0.tianditu.com/cva_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=cva&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default.jpg&tk=a7210ed6e9196896338564cb37289650",
      layer: "tdtAnnoLayer",
      style: "default",
      format: "image/jpeg",
      tileMatrixSetID: "GoogleMapsCompatible",
      show: false
    }));
    // const viewer = new Cesium.Viewer(this.cesiumContainer,{
    //   geocoder:false,
    //   homeButton:false,
    //   sceneModePicker:false,
    //   baseLayerPicker:false,
    //   navigationHelpButton:false,
    //   animation:false,
    //   creditContainer:"credit",
    //   timeline:false,
    //   fullscreenButton:false,
    //   vrButton:false,
    //   imageryProvider : new Cesium.WebMapTileServiceImageryProvider({
    //     url: "http://t0.tianditu.com/img_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=img&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default&format=tiles&tk=a7210ed6e9196896338564cb37289650",
    //     layer: "tdtBasicLayer",
    //     style: "default",
    //     format: "image/jpeg",
    //     tileMatrixSetID: "GoogleMapsCompatible",
    //     show: false
    //   })
    // });
    
    // viewer.imageryLayers.addImageryProvider(new Cesium.WebMapTileServiceImageryProvider({
    //   url: "http://t0.tianditu.com/cia_w/wmts?service=wmts&request=GetTile&version=1.0.0&LAYER=cia&tileMatrixSet=w&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&style=default.jpg&tk=a7210ed6e9196896338564cb37289650",
    //   layer: "tdtAnnoLayer",
    //   style: "default",
    //   format: "image/jpeg",
    //   tileMatrixSetID: "GoogleMapsCompatible",
    //   show: false
    // }));

    let dataDetail;
    $.ajax({
      type: 'get',
      //url: 'api/map/business/getMapInfo?&tableId=1&type=B&lat1=22.906494140625&lat2=30.904541015625&lng1=97.679443359375&lng2=115.718994140625&searchType=3',
      url:'api/map/business/getNwfData?&tableId=41&type=1&lat1=22.906494140625&lat2=30.904541015625&lng1=97.679443359375&lng2=115.718994140625&searchType=3',
      //data: { dataid: item.dataid, tableId: thisLayer.tableId },
      async: false,
      success: function(data) {
        dataDetail = data;
      },
    });
    //柱状
    var czml = [
      { "id": "document", "name": "box", "version": "1.0" ,
        "clock": {
          "interval": "2019-01-01T00:00:00Z/2019-01-01T00:00:05Z",
          "currentTime": "2019-01-01T00:00:00Z",
          "multiplier": 1,
          "range": "CLAMPED",
          "step": "SYSTEM_CLOCK_MULTIPLIER"
        }

      },
      // {
      //   "id": "shape2", "name": "Red box with black outline",
      //   "position":{
          
      //     //"epoch" : "2019-01-01T00:00:00Z",
      //     "cartographicDegrees" : [
      //       106.703419,26.901738,0.0,
      //     ],
      //     // "interpolationAlgorithm":"LAGRANGE",
      //     // "interpolationDegree":1,
      //   },
      //   "cylinder": {
      //     length:
      //       {
      //         "epoch" : "2019-01-01T00:00:00Z",
      //         "number":[
      //           0,5000,
      //           5,20000,],
      //         "interpolationAlgorithm":"LAGRANGE",
      //         "interpolationDegree":1,
      //       },
      //     topRadius: 800,
      //     bottomRadius: 2000,
      //     //"dimensions": { "cartesian": [4000.0, 4000.0, 50000.0] },
      //     "material": { "solidColor": { "color": { "rgba": [255, 0, 0, 180] } } },
      //     //"outline": true, "outlineColor": { "rgba": [0, 0, 0, 255] }
      //   }
      // }
    ]; 
    const arrRgba=[
      [1, 152, 189,255],
      [73, 227, 206,255],
      [216, 254, 181,255],
      [254, 237, 177,255],
      [254, 173, 84,255],
      [209, 55, 78,255],
    ]
    let i=0;
    //console.log(Cesium.JulianDate.fromIso8601('2019-01-01'));
    dataDetail.data/*.filter(item=>i++<100)*/.map(data=>{
      // let t=data.completed_cnt*1.0/data.total_cnt;
      // let agba=arrRgba[parseInt(t*5)];
      let t=data.total_cnt/300;
      let tC=t;
      if(tC>1) tC=1;
      let agba=arrRgba[parseInt(tC*5)];
      let length=parseInt(t*30)*1000.0;
      let czmlObj= {
        "id": data.dataid, "name": data.dataname,
        "position": {
          "cartographicDegrees": [data.geolng,data.geolat, length/2] ,
          },
        "cylinder": {
          length,
          topRadius: 1000,
          bottomRadius: 1000,
          //"dimensions": { "cartesian": [4000.0, 4000.0, parseInt(Math.random()*50)*1000.0] },
          "material": { "solidColor": { "color": { "rgba": agba } } },
          //"outline": true, "outlineColor": { "rgba": [0, 0, 0, 255] }
        }
      }
      return czmlObj;
    }).forEach(data=>czml.push(data));

    //视角
    viewer.scene.globe.enableLighting  = true;
    //106.703419,26.901738
    var initialPosition = Cesium.Cartesian3.fromDegrees(109.61181640625001, 23.252197265625, 453000);
    var initialOrientation = new Cesium.HeadingPitchRoll.fromDegrees(-35, -45, 0.0716951918898415);
    //viewer.scene.camera.flyTo({
    viewer.scene.camera.setView({
        destination: initialPosition,
        orientation: initialOrientation,
        endTransform: Cesium.Matrix4.IDENTITY
    });

    //console.log(czml);
      // dataDetail.forEach(data => {
      //   let czmlObj= {
      //     "id": data.dataid, "name": "Red box with black outline",
      //     "position": { "cartographicDegrees": [106.703419,26.901738, 30000.0] },
      //     "box": {
      //       "dimensions": { "cartesian": [4000.0, 4000.0, 50000.0] },
      //       "material": { "solidColor": { "color": { "rgba": [255, 0, 0, 180] } } },
      //       //"outline": true, "outlineColor": { "rgba": [0, 0, 0, 255] }
      //     }
      //   }
      // });
    setTimeout(function(){
      var dataSourcePromise = Cesium.CzmlDataSource.load(czml);
      viewer.dataSources.add(dataSourcePromise);
      // setTimeout(function(){
      //   $("div.cesium-viewer-animationContainer>svg>g",$("#mapDiv")).lastElementChild.children[2].dispatchEvent(new MouseEvent('click'));
      // },10000)
      // var start = Cesium.JulianDate.fromIso8601('2019-01-01');
      // var end = Cesium.JulianDate.fromIso8601('2019-01-02');

      // viewer.timeline.zoomTo(start, end);

      // var clock = viewer.clock;
      // clock.startTime = start;
      // clock.endTime = end;
      // clock.currentTime = start;
      // clock.clockRange = Cesium.ClockRange.LOOP_STOP;
      // clock.multiplier = 1;

      // // 起始时间
      // let start = Cesium.JulianDate.fromDate(new Date(2017,7,11));
      // // 结束时间
      // let stop = Cesium.JulianDate.addSeconds(start, 360, new Cesium.JulianDate());

      // // 设置始时钟始时间
      // view.clock.startTime = start.clone();
      // // 设置时钟当前时间
      // view.clock.currentTime = start.clone();
      // // 设置始终停止时间
      // view.clock.stopTime  = stop.clone();
      // // 时间速率，数字越大时间过的越快
      // view.clock.multiplier = 10;
      // // 时间轴
      // view.timeline.zoomTo(start,stop);
      // // 循环执行
      // view.clock.clockRange = Cesium.ClockRange.LOOP_STOP;
    },1000)
     //viewer.zoomTo(dataSourcePromise); //定位到柱子
  }

  getMap(){
    return <div id="mapDiv" style={{ width: '100%', height: '800px' }} ref={ element => this.cesiumContainer = element } />;
  }

  render() {
    const {
      location: { pathname },
    } = this.props;
    const map = this.getMap();
    return (
      <React.Fragment>
        <DocumentTitle title="测试" />
        <div>
          <div>
            <h1>欢迎你</h1>
          </div>
          <div>
            <div id="credit" style={{ display:"none"}}></div>
            {map}
          </div>
        </div>
      </React.Fragment>
    );
  }
}
export default Index;
