<template>
  <div id="echart" style="width: 100%; height: 100%"></div>
</template>
<script>
import * as echarts from "echarts";
import axios from "axios";

export default {
  data() {
    return {
      option: {
        title: { text: "生活垃圾产生量" },
        tooltip: {},
        xAxis: {
          data: []
        },
        yAxis: {},
        series: [
          {
            name: "产生量（万吨）",
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
      myechart.setOption(this.option);
    },
    async getGarbageData() {
      const { data: res } = await axios({
        method: "GET",
        url: "http://localhost:8088/tanghao/api/getGarbageData"
      });
       this.option.xAxis.data=res.years
       this.option.series[0].data=res.numbers
    }
  },
  created() {
    this.getGarbageData();
  },
  mounted() {
    setTimeout(()=>{
      this.getCharts()
    },1000)
   
  }
};
</script>
<style lang='less'>
</style>