1.使用柱状图，Y轴数值比较大，会出现数值被遮挡的情况，解决办法是配置项中添加
```javascript
grid:{
  left:30
}
```
2.Y轴显示数值太大也别扭，直接把单位改了应该更好，使用axisLabel的formatter即可
```javascript
axisLabel:{
  formatter:function(value,index){
    return value/10000
  }
}
```
3.柱状图的数据是获取的远程数据，通过axios获取到数据之后更新数据总是不起作用，后来用watch解决
```html
<template>
  <div>
    <Divider>支付清单分析</Divider>
    <tab bar-position="top" v-model="index">
      <tab-item selected>按报销单类型</tab-item>
      <tab-item>按收款人类型</tab-item>
    </tab>
    <div id="tab-cnt-box">
      <div class="tab-swiper" v-show="index===0">
        <div class="clearfix group-title">单据类型:</div>
        <div class="clearfix group-cnt">
          <checker
            v-model="billtype"
            type="checkbox"
            default-item-class="checker-item"
            selected-item-class="checker-item-selected"
            >
            <checker-item v-for="(d,i) in BillTypes" :key="i" :value="d">{{d}}</checker-item>
          </checker>
        </div>
        <div class="clearfix group-title">时间范围:</div>
        <div class="clearfix group-cnt">
          <input type="text" name="starttime" @click="chooseDate" v-model="starttime" readonly onfocus="this.blur()"> --- <input type="text" name="endtime" @click="chooseDate" v-model="endtime" readonly onfocus="this.blur()">
        </div>
        <x-button type="primary" action-type="button" @click.native="baoxiaodanStat">按报销单类型统计</x-button>
        <div id="charts-box">
          <div id="barChartBox" :style="{width: '350px', height: '350px',padding:'0 0 0 20px'}"></div>
          <div id="pieChartBox" style="display:none"></div>
        </div>
      </div>
      <div class="tab-swiper vux-center" v-show="index===1">
      </div>
    </div>

  </div>
</template>

<script>
import { XButton,Divider,Tab,TabItem,Checker, CheckerItem,Datetime } from 'vux'
import { DateFormat,DateAdd,CurrentDate } from '../../utils/datetime'
// 引入基本模板
let echarts = require('echarts/lib/echarts')
// 引入柱状图组件
require('echarts/lib/chart/bar')
// 引入提示框和title组件
require('echarts/lib/component/tooltip')
require('echarts/lib/component/title')
require('echarts/lib/component/legend');
require('echarts/lib/component/markLine');
let date = new Date();

let chartsbox = document.getElementById('charts-box')
// let elPieChart = document.getElementById('pieChart')
let elbarChartBox = document.getElementById('barChartBox')
const dtNow = CurrentDate()
export default {
  components:{ XButton,Divider, Tab,TabItem,Checker, CheckerItem,Datetime },
  data () {
    return {
      index :0,
      billtype:['差旅费报销单'],
      starttime:DateFormat(DateAdd(date,'m','-1')),
      endtime: dtNow,
      isShowCharts:false,
      BillTypes: {
        'chailv':'差旅费报销单',
        'tongyong':'通用报销单',
        'laowu':'劳务报销单',
      },
      baoxiaodanData:{}
    }
  },
  mounted(){
  },
  watch:{
    baoxiaodanData(){
      this.drawbarChart()
    }
  },
  methods:{
    //报销单分析
    baoxiaodanStat() {
      if(this.billtype.length<1) {
        this.$vux.alert.show('请至少选择一项单据类型')
        return
      }
      if(this.starttime.length<1 || this.endtime.length<1) {
        this.$vux.alert.show('请正确选择时间范围')
        return
      }

      let parms = {'billtype':this.billtype,'starttime':this.starttime,'endtime':this.endtime}
      let queryUrl = '/yyapi/zhifuqingdan_baoxiaodan_single'
      if(this.billtype.length>1) {
        queryUrl = '/yyapi/zhifuqingdan_baoxiaodan_multi'
      }
      this.$http.get(queryUrl,{params:parms}).then(
        res => {
          let data = res.data
          this.baoxiaodanData={monthData:data.smonth,moneyData:data.summoney}
          // this.$vux.loading.hide()
        }
      )
    },
    chooseDate (event) {
      const self = this
      this.$vux.datetime.show({
        cancelText: '取消',
        confirmText: '确定',
        format: 'YYYY-MM-DD',
        onConfirm (val) {
          event.target.name==='starttime'?(self.starttime = val):(self.endtime = val)
        }
      })
    },
    drawbarChart() {
      // 基于准备好的dom，初始化echarts实例
      let barChartBox = echarts.init(document.getElementById('barChartBox'))
      // 绘制图表
      barChartBox.setOption({
          tooltip: {
            trigger: 'axis',
          },
          grid: {
            left: 30,
          },
          xAxis: {//月份
              data: this.baoxiaodanData.monthData
          },
          yAxis: { //该项在相应月份的数值
            name: '金额(万元)',
            scale: false,
            axisLabel:{
              formatter:  function (value, index) {
                return value/10000
              }
            },
          },
          series: [{
              name: '按月份统计',
              type: 'bar',
              data: this.baoxiaodanData.moneyData
          }]
      });
    },//drawbarChart
  }
}
</script>

<style scoped>
#charts-box {width: 100%;height: 400px}
.group-title {font-size: 14px}
.group-cnt {font-size: 14px}
.group-cnt input {width:45%;height:22px;line-height:22px;text-align: center; border-radius: 3px;border:1px solid #ccc;background-color: #fff;ime-mode: disabled}
.checker-item {
  width: 100px;
  height: 26px;
  line-height: 26px;
  text-align: center;
  border-radius: 3px;
  border: 1px solid #ccc;
  background-color: #fff;
  margin-right: 6px;
}
.checker-item-selected {
  background: #ffffff url(../../assets/img/active.png) no-repeat right bottom;
  border-color: #ff4a00;
}
</style>
```
