<template>
  <div class="hello">
    <!-- 初始化Echarts需要个有宽高的盒子 -->
    <div ref="mapbox"></div>
  </div>
</template>

<script>
import echarts from "echarts";
import "echarts/map/js/china.js";
import jsonp from "jsonp";

//使用地图前先注册
const option = {
  title: {
    text: "疫情地图",
    link: "https://www.baidu.com",
    subtext: "累计确诊",
    sublink: "",
    left: "400px",
    top: "80px"
  },
  series: [
    {
      name:"切诊人数",
      type: "map", //渲染的是地图
      map: "china", //渲染的地图的是中国地图
      label: {
        show: true, //是否显示标签(汉字)
        color: "#000",
        fontSize: 12
      },
      itemStyle: {
        areaColor: "#ccc", //控制地图板块的背景颜色
        border: "#000" //控制地图板块的边框颜色
      },
      roam: true,
      zoom: 1.2, //控制放大缩小
      emphasis: {
        //鼠标悬停时的样式
        label: {
          color: "#000",
          fontSize: 12
        },
        itemStyle: {
          areaColor: "#c7fffd", //控制地图板块的背景颜色
          border: "#000" //控制地图板块的边框颜色
        }
      },
      data: [
        //展示后台数据
      ]
    }
  ],
  visualMap: [
    {
      type: "piecewise", //设置样式:分段的
      show: true,
      // splitNumber: 4//设置分为几段
      pieces: [
        //具体分段数据
        { min: 10000 },
        { min: 1000, max: 9999 },
        { min: 100, max: 999 },
        { min: 10, max: 99 },
        { min: 1, max: 9 }
      ],
      // align:'right'//控制分段图标和文字位置
      orient: "vertical", //展示方向横排(horizontal)/竖排(vertical)
      bottom: "55px",
      right: "300px", //控制分段偏移
      // showLabel:'true'//是否展示文字
      inRange: {
        // symbol: 'reat',//分段的形状reat 块状  circle 圆形
        color: ["#ffe5db", "#ff9985", "#f57567", "#e64546", "#b80909"]
      },
      itemWidth: 35,
      itemHeight: 20
    }
  ],
  tooltip:{
    trigger:'item'
  },
  toolbox: {
    show: true,
    orient: 'vertical',
    left: 'right',
    top: 'center',
    feature: {
      dataView: {readonly: false},
      restore:{},
      saveAsImage: {}
    }
  }
};
export default {
  name: "HelloWorld",
  mounted() {
    this.getData();
    this.mychart = echarts.init(this.$refs.mapbox);
    // this.mychart.setOption(option); 
  },
  methods: {
    getData() {
      jsonp(
        "https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427",
        {},
        (err, data) => {
          if (!err) {
            // console.log(data);
            let list = data.data.list.map(item => ({
              name: item.name,
              value: item.value
            }));
            option.series[0].data = list;
            this.mychart.setOption(option);
          } else {
            console.log("data fail");
          }
        }
      );
    }
  }
};
</script>

<style scoped>
.mapbox {
  width: 1200px;
  height: 800px;
  margin: 0 auto;
}
</style>
