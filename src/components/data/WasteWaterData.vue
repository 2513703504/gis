<template>
  <div id="echart" style="width: 100%; height: 100%"></div>
</template>
<script>
import * as echarts from "echarts";
import axios from "axios";
import { watch } from 'fs';
import { setTimeout } from 'timers';

export default {
  data() {
    return {
      option: {
        title: { text: "污水排放量统计预测" },
        tooltip: {},
        xAxis: {
          data: []
        },
        yAxis: {},
        series: [
          {
            name: "排放量（万吨）",
            type: "bar",
            data: []
          }
        ]
      } 
    };
  },
  methods: {
    getCharts() {
      var myechart = echarts.init(document.getElementById("echart"));
      myechart.clear();
      myechart.setOption(this.option);
    },
    async getWasteWaterData() {
      const { data: res } = await axios({
        method: "GET",
        url: "http://localhost:8088/tanghao/api/getWaterData"
      });
       this.option.xAxis.data=res.years
       this.option.series[0].data=res.numbers

    }
  },
  created() {
    this.getWasteWaterData();
  },
  mounted() {
    setTimeout(()=>{
      this.getCharts()
    },1000)
   
  },
};
</script>
<style lang='less'>
</style>