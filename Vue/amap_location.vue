<template>
  <div class="map">
    <v-header
      :title="title"
      :gohome="true"
    ></v-header>
    <div
      id="container"
      :class="active"
    ></div>
    <div
      class="info"
      v-show="isShowPointInfo"
    >
      <span class="i-name">定位结果：{{ lng }},{{ lat }}</span>
      <br>
      <span class="i-type">定位类别：{{ type }}</span>
      <br>
      <!-- <div>坐标：{{clockParams}}</div> -->
      <div>偏移范围：{{jd}}</div>
    </div>
    <div id="box">
      <div class="time">{{time}}</div>
      <van-button
        type="info"
        size="mini"
        class="signBtn"
        v-if="xxgStatus == 'normal'"
        @click="startPosition"
      >开始巡线</van-button>
      <van-button
        type="info"
        size="mini"
        class="signBtn"
        v-else
        @click="stopPosition"
      >停止巡线</van-button>
    </div>
    <div id="address">
      <span v-if="isLocation">系统正在定位中</span>
      <span v-else>{{address}}</span>

    </div>
    <!-- <div>
      <p>polyline:{{polylinePath}}</p>
      <p>path:{{path}}</p>
    </div> -->
    <van-overlay
      :show="isShowMapApp"
      @click="isShowMapApp = false"
    />
  </div>
</template>

<script>
  import { mapActions, mapMutations, mapState } from 'vuex';
  import { getLoc, setLoc, formatDate, getPostData, getParams } from '@/utils/common.js';
  import { Toast } from 'vant';
  export default {
    data() {
      //这里存放数据
      return {
        title: '地图',
        name: '',
        active: '',
        index: '0',
        isShowPointInfo: false,
        isShowMapApp: false,
        hasMap: true, //本地是否安装地图APP
        lat: '',
        lng: '',
        markerIcon: require('../../assets/nav.png'),
        polyline: {}, //轨迹线段
        polylinePath: [], // 巡线工坐标轨迹
        clockParams: [],
        initMarker: {},
        markerStart: {}, //开始定位
        markerEnd: {}, //结束定位
        time: '',
        type: '',
        isMove: '',
        jd: '',
        address: '',
        isLocation: true,
        locationType: 'html5'
      };
    },
    props: [],
    //监听属性 类似于data概念
    computed: {
      ...mapState(['pointArr', 'xxgStatus'])
    },
    //监控data中的数据变化
    watch: {},
    //生命周期 - 创建完成（可以访问当前this实例）
    created() { },
    //生命周期 - 挂载完成（可以访问DOM元素）
    mounted() {
      //判断是否有巡线任务在进行
      console.log(this.xxgStatus);
      this.initMap();
      if(this.xxgStatus == 'normal') {
        this.polylinePath = [];
        this._pointArr([]);
      } else {
        let points = Object.assign({}, this.pointArr);
        this.polylinePath = [];
        for(let i in points) {
          this.polylinePath.push(points[i]);
        }
        console.log(this.polylinePath);
      }
      setInterval(() => {
        this.time = new Date().toLocaleString() + ' 星期' + '日一二三四五六'.charAt(new Date().getDay());
      }, 1000)
    },
    //方法集合
    methods: {
      ...mapMutations(['_pointArr', '_xxgStatus']),
      ...mapActions(['_getInfo']),
      // 初始化
      initMap() {
        let _this = this;
        AMap.plugin('AMap.Geolocation', function() {
          var geolocation = new AMap.Geolocation({
            enableHighAccuracy: true,
            timeout: 10000,
            buttonPosition: 'RB',
            buttonOffset: new AMap.Pixel(10, 20),
            zoomToAccuracy: true,
            useNative: true, //是否使用安卓定位sdk来进行定位
          });
          geolocation.getCurrentPosition();
          AMap.event.addListener(geolocation, 'complete', onComplete);
          AMap.event.addListener(geolocation, 'error', onError);
          // data是具体的定位信息
          function onComplete(data) {
            console.log(data);
            const latitude = data.position.lat; // 纬度
            const longitude = data.position.lng; // 经度

            if(data.location_type == 'ip') {
              //   alert('ip定位');
              _this.plusFn();
              console.log(123);
            } else {
              _this.lat = latitude;
              _this.lng = longitude;
              _this.map = new AMap.Map('container', {
                zoom: 16, //级别
                center: [longitude, latitude] //中心点坐标
              });
              _this.map.addControl(geolocation);
              _this.type = data.location_type;

              if(data.accuracy) {
                _this.jd = data.accuracy;
              };
              _this.isMove = data.isConverted ? '是' : '否';
              _this.address = data.formattedAddress;
              _this.isShowPointInfo = true;
              _this.isLocation = false;

              //判断是否有巡线任务在执行
              if(_this.xxgStatus == 'normal') {
                _this.markerStart = new AMap.Marker({
                  position: new AMap.LngLat(longitude, latitude) // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
                });
                _this.markerStart.setMap(_this.map);
              } else {
                console.log(_this.polylinePath[0]);
                let startLng = _this.polylinePath[0].lng;
                let startLat = _this.polylinePath[0].lat;
                _this.addStart(startLng, startLat);
              }
            }
          }
          function onError(data) {
            _this.plusFn();
          }
        });
      },
      // 获取定位
      getPosition() {
        let _this = this;
        AMap.plugin('AMap.Geolocation', function() {
          var geolocation = new AMap.Geolocation({
            // 是否使用高精度定位，默认：true
            enableHighAccuracy: true,
            //是否将定位结果转换为高德坐标
            convert: true,
            // 设置定位超时时间，默认：无穷大
            timeout: 10000,
            //浏览器原生定位的缓存时间
            maximumAge: 0,
            // 定位按钮的停靠位置的偏移量，默认：Pixel(10, 20)
            buttonOffset: new AMap.Pixel(10, 20),
            //  定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
            zoomToAccuracy: true,
            //  定位按钮的排放位置,  RB表示右下
            buttonPosition: 'RB'
          });

          geolocation.getCurrentPosition();
          // 事件监听 complete
          AMap.event.addListener(geolocation, 'complete', onComplete);
          // 事件监听 error
          AMap.event.addListener(geolocation, 'error', onError);

          function onComplete(data) {
            // data是具体的定位信息

            _this.lat = data.position.lat;
            _this.lng = data.position.lng;

            console.log(_this.markerStart, _this.polylinePath.length);
            // 判断是否是刚开始巡线
            if(_this.polylinePath.length == 0) {
              _this.clearOverlays();
              console.log(_this.lng, _this.lat);
              _this.addStart(_this.lng, _this.lat);
            }
            console.log(_this.lng, _this.lat);
            // 需要将数据存储到clockParams 字段里
            _this.clockParams.push({ clockCoordinate: _this.lng + ',' + _this.lat, clockTime: formatDate(new Date().getTime() - 6 * 24 * 3600 * 1000, 1) });
            // 缓存
            setLoc('clockParams', _this.clockParams);
            console.log('data:' + _this.clockParams);

            //polylinePath为轨迹的所有坐标点
            _this.polylinePath.push({ lng: _this.lng, lat: _this.lat });
            let pointObj = Object.assign({}, _this.polylinePath);
            console.log(pointObj);
            let points = [];
            for(let i in pointObj) {
              points.push(pointObj[i]);
            }
            //更新轨迹坐标
            _this._pointArr(points);
            console.log(_this.lng + '-' + _this.lat);
          }

          function onError(data) {
            // 定位出错
            alert('error:' + data)
          }
        });
      },
      // 开始定位如果是ip定位，则使用plus获取坐标
      getPlusFn() {
        let _this = this;
        plus.geolocation.getCurrentPosition(function(p) {
          //   alert('plus Geolocation\nLatitude:' + p.coords.latitude + '\nLongitude:' + p.coords.longitude + '\nAltitude:' + p.coords.altitude);
          _this.lat = p.coords.latitude;
          _this.lng = p.coords.longitude;

          // 判断是否是刚开始巡线
          if(_this.polylinePath.length == 0) {
            _this.clearOverlays();
            console.log(_this.lng, _this.lat);
            _this.addStart(_this.lng, _this.lat);
          }
          console.log(_this.lng, _this.lat);
          // 需要将数据存储到clockParams 字段里
          _this.clockParams.push({ clockCoordinate: _this.lng + ',' + _this.lat, clockTime: formatDate(new Date().getTime() - 6 * 24 * 3600 * 1000, 1) });
          // 缓存
          setLoc('clockParams', _this.clockParams);
          console.log('data:' + _this.clockParams);

          //polylinePath为轨迹的所有坐标点
          _this.polylinePath.push({ lng: _this.lng, lat: _this.lat });
          let pointObj = Object.assign({}, _this.polylinePath);
          console.log(pointObj);
          let points = [];
          for(let i in pointObj) {
            points.push(pointObj[i]);
          }
          //更新轨迹坐标
          _this._pointArr(points);
          console.log(_this.lng + '-' + _this.lat);
        }, function(e) {
          alert('Geolocation error: ' + e.message);
        });
      },
      //plus 初始化获取定位
      plusFn() {
        this.locationType = 'ip';
        let _this = this;
        var geolocation = new AMap.Geolocation({
          // 是否使用高精度定位，默认：true
          enableHighAccuracy: true,
          //是否将定位结果转换为高德坐标
          convert: true,
          // 设置定位超时时间，默认：无穷大
          timeout: 10000,
          //浏览器原生定位的缓存时间
          maximumAge: 0,
          // 定位按钮的停靠位置的偏移量，默认：Pixel(10, 20)
          buttonOffset: new AMap.Pixel(10, 20),
          //  定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
          zoomToAccuracy: true,
          //  定位按钮的排放位置,  RB表示右下
          buttonPosition: 'RB'
        });
        plus.geolocation.getCurrentPosition((p) => {
          //   alert('Geolocation\nLatitude:' + p.coords.latitude + '\nLongitude:' + p.coords.longitude + '\nAltitude:' + p.coords.altitude);

          const latitude = p.coords.latitude; // 纬度
          const longitude = p.coords.longitude; // 经度
          _this.address = p.addresses;
          _this.type = p.coordsType;
          if(p.coords.accuracy) {
            _this.jd = p.coords.accuracy;
          };

          //   alert('plus具体的定位信息:', p.coords.longitude, p.coords.latitude);
          _this.lat = p.coords.latitude;
          _this.lng = p.coords.longitude;
          _this.map = new AMap.Map('container', {
            zoom: 16, //级别
            center: [_this.lng, _this.lat] //中心点坐标
          });
          _this.map.addControl(geolocation);
          _this.initMarker = new AMap.Marker({
            position: new AMap.LngLat(_this.lng, _this.lat) // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
          });
          _this.initMarker.setMap(_this.map);
          //   alert(_this.lng, _this.lat);
          // _this.isMove = data.isConverted ? '是' : '否';

          _this.isShowPointInfo = true;
          _this.isLocation = false;

        }, function(e) {
          alert('Geolocation error: ' + e.message);
        });
      },
      //开始巡线
      startPosition() {
        //判断地图上是否有标记或覆盖物
        if(this.polylinePath.length > 0) {
          console.log('有数据，先清除');
          this.clearOverlays();
        }
        this.polylinePath = [];
        this._pointArr([]);
        this._xxgStatus('start');

        //启动定时器
        window.timer = setInterval(() => {
          if(this.locationType == 'ip') {
            this.getPlusFn();
          } else {
            this.getPosition();
          }
        }, 10000);
        // window.uploadTimer = setInterval(() => {
        //   this.uploadPoints();
        // }, 60000);
      },
      //停止巡线
      stopPosition() {
        // let path = [[108.84498596, 34.18127568], [108.844986, 34.181276], [108.88137817, 34.17644723]];
        let path = [];
        console.log(this.polylinePath);
        // 巡线状态变为结束
        this._xxgStatus('normal');
        this.polylinePath.forEach((item) => {
          path.push([item.lng, item.lat]);
          //   console.log(path);
        });
        let lastLng = this.polylinePath[this.polylinePath.length - 1].lng;
        let lastLat = this.polylinePath[this.polylinePath.length - 1].lat;

        this.addEnd(lastLng, lastLat);
        //绘制轨迹
        this.drawPath(path);
        // this.uploadPoints();
        clearInterval(window.timer);
        // clearInterval(window.uploadTimer);

        this._pointArr([]);
      },
      //添加开始标志
      addStart(lng, lat) {
        this.markerStart = new AMap.Marker({
          position: new AMap.LngLat(lng, lat),
          offset: new AMap.Pixel(-13, -30),
          icon: require('../../assets/map/start.png') // 添加 Icon 图标 URL
        });
        this.markerStart.setMap(this.map);
      },
      //添加结束标志
      addEnd(lng, lat) {
        this.markerEnd = new AMap.Marker({
          position: new AMap.LngLat(lng, lat),
          offset: new AMap.Pixel(-13, -30),
          icon: require('../../assets/map/end.png') // 添加 Icon 图标 URL
        });
        this.markerEnd.setMap(this.map);
      },
      //添加轨迹
      drawPath(path) {
        this.polyline = new AMap.Polyline({
          path: path,
          isOutline: true,
          outlineColor: '#ffeeff',
          borderWeight: 3,
          strokeColor: '#ff0000',
          strokeOpacity: 1,
          strokeWeight: 3,
          // 折线样式还支持 'dashed'
          strokeStyle: 'solid',
          // strokeStyle是dashed时有效
          strokeDasharray: [10, 5],
          lineJoin: 'round',
          lineCap: 'round',
          zIndex: 50
        });

        this.polyline.setMap(this.map);
      },
      // 移除覆盖物
      clearOverlays() {
        this.map.clearMap();
      },
      //上传数据
      uploadPoints() {
        // console.log(JSON.stringify(this.clockParams));
        this.clockParams = getLoc('clockParams');
        const params = getPostData('insertClockInfo', [getLoc('userInfo').userID, JSON.stringify(this.clockParams)]);
        console.log(params, 666);
        this.clockParams = [];
        setLoc('clockParams', []);
        // this._getInfo({
        //   ops: params,
        //   method: 'post',
        //   api: 'insertClockInfo',
        //   callback: (res) => {
        //     var div = document.createElement('div');
        //     div.innerHTML = res;
        //     var listInfoData = div.querySelector('return').innerHTML;
        //     console.log(listInfoData);
        //     return listInfoData;
        //     //清空数据
        //     this.clockParams = [];
        //     setLoc('clockParams', []);
        //   }
        // });
      }
    },
    updated() {
      //this.init();
    }, //生命周期 - 更新之后
    activated() { } //如果页面有keep-alive缓存功能，这个函数会触发
  };
</script>
<style lang="less" scoped>
  .map {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    width: 100%;
    overflow: hidden;
    font-size: 0.3rem;
    .mapAppList {
      position: absolute;
      top: 45%;
      left: 50%;
      width: 85%;
      background-color: #fff;
      border-radius: 4px;
      -webkit-transform: translate3d(-50%, -50%, 0);
      transform: translate3d(-50%, -50%, 0);
      backface-visibility: hidden;
      -webkit-transition: 0.3s;
      transition: 0.3s;
      z-index: 2042;
      ul {
        li {
          text-align: left;
          line-height: 0.8rem;
          margin: 0 0.1rem;
          padding: 0 0.1rem;
          border-bottom: 1px solid #e2e2e2;
        }
        li:last-child {
          border-bottom: none;
        }
        li:active {
          background-color: #eee;
        }
      }
    }
  }
  #container {
    position: relative;
    height: 50%;
    width: 100%;
  }
  #container.active {
    bottom: 0;
  }
  .info {
    padding: 0.35rem 0.65rem;
    margin-bottom: 1rem;
    border-radius: 0.25rem;
    position: fixed;
    top: 1.4rem;
    background-color: white;
    width: auto;
    border-width: 0;
    right: 1rem;
    text-align: left;
    box-shadow: 0 2px 6px 0 rgba(114, 124, 245, 0.5);
    .i-name {
      white-space: pre-wrap;
      float: left;
    }
  }
  .time {
    height: 1rem;
    line-height: 1rem;
  }
  .signBtn {
    margin: 0.1rem auto;
    width: 2.8rem;
    height: 2.8rem;
    border-radius: 1.4rem;
    box-shadow: 0px 0px 8px #25a4ff;
    font-size: 0.4rem;
  }
  #address {
    height: 1rem;
    line-height: 1rem;
    font-size: 0.2rem;
  }
</style>
