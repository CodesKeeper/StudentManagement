<template>
  <div class="trave-map">
    <div id="main"></div>
  </div>
</template>
<script>
import geoJson from "@/assets/data.json";
import { travel } from "@/api/api";
import map from '@/utils/map.js'
import echarts from 'echarts'
import { onMounted } from 'vue'
export default {
  setup() {
    const drawMap = function(points, linesData) {
      let myChart = echarts.init(document.getElementById("main"));
      echarts.registerMap("china", geoJson); // 注册可用的地图,必须包括geo组件或者map图表类型的时候才可以使用

      // 关于地图的散点配置
      let pointsObj = {
        type: "effectScatter", //散点图
        coordinateSystem: "geo",
        showEffectOn: "render",
        symbolSize: 10, //散点大小
        zlevel: 1,
        data: points,
        rippleEffect: {
          // 涟漪特效相关配置
          period: 15,
          scale: 4,
          brushType: "fill",
        },
      }
      // 关于地图中线路动画效果设置
      let linesObj = {
        type: 'lines',
        zlevel: 2,
        effect: {
            show: true,
            period: 4,
            symbol: 'arrow',
            symbolSize: 7,
            trailLength: 0.4
        },
        lineStyle: {
            normal: {
                color: '#1DE9B6',
                width: 1,
                opacity: 0.1,
                curveness: 0.3
            }
        },
        data: linesData
      }
      map.series[1] = pointsObj
      map.series[2] = linesObj
      let option = map
      myChart.setOption(option);
    }
    onMounted(
      travel().then((res) => {
      if (res.data.status === 200) {
          let { points, linesData } = res.data.data;
          drawMap(points, linesData);
        }
      })
    )
  }
};
</script>
<style lang="less">
.trave-map {
  width: 100%;
  #main {
    width: 100%;
    height: 600px;
  }
}
</style>
