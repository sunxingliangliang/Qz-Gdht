<template>
  <div>
  
    <!-- 用户画像 -->
    <div>
      <div :class="$style.f_hx1">
        <span :class="$style.f_mc">{{mc}}</span>
        <div id="myChart" style="position: static; width:100%;height:300px;"></div>
      </div>
      <div :class="$style.f_hx1">
        <span :class="$style.f_mc">{{mc1}}</span>
        <div id="myChart1" style="position: static; width:100%;height:300px;"></div>
      </div>
      <div :class="$style.f_hx1">
        <span :class="$style.f_mc">{{mc2}}</span>
        <div id="myChart2" style="position: static; width:100%;height:300px;"></div>
      </div>
      <div :class="$style.f_hx1">
        <span :class="$style.f_mc">{{mc3}}</span>
        <div id="myChart3" style="position: static; width:100%;height:300px;"></div>
      </div>
      <div :class="$style.f_hx1">
        <span :class="$style.f_mc">{{mc4}}</span>
        <div id="myChart4" style="position: static; width:100%;height:300px;"></div>
      </div>
      <div :class="$style.f_hx1">
        <span :class="$style.f_mc">{{mc5}}</span>
        <div id="myChart5" style="position: static; width:100%;height:300px;"></div>
      </div>
      <div :class="$style.f_hx2">
        <span :class="$style.f_mc">{{mc6}}</span>
        <div id="myChart6" style="position: static; width:100%;height:500px;"></div>
      </div>
      <div :class="$style.f_hx2">
        <span :class="$style.f_mc">{{mc7}}</span>
        <div id="myChart7" style="position: static; width:100%;height:500px;"></div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
export default {
  props: ['message'],
  data () {
    return {
      id: ''
    }
  },
  computed: {
    ...mapGetters(['forList'])
  },
  mounted () {
    console.log(this.$store.getters.forLive)
    this.id = this.$store.getters.forLive
    // this.id = this.$store.state.id
    // console.log(this.id)
    // this.drawLine();
    // this.drawLine1();
    // this.drawLine2();
    // this.drawLine3();
    // this.drawLine4();
    // this.drawLine5();
    this.getData()
  },
  methods: {
    getData () {
      this.$http.get(`modules/persona/dataView?orderId=${this.id}&type=catering`).then(res => {
        var { code, data } = res.data
        if (code === 1000) {
          console.log(data)
        }
      }).catch((err) => {
        console.log('错误信息' + err)
      })
    },
    drawLine () {
      console.log(this.message)
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById("myChart"));
      // 绘制图表
      let option = {
        title: {
          text: "男女比例",
          x: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: "left",
          data: ["男", "女"]
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "60%"],
            data: [
              { value: 123, name: "男" },
              { value: 411, name: "女" }
            ],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            }
          }
        ]
      };
      myChart.setOption(option);
    },
    drawLine1 () {
      // 基于准备好的dom，初始化echarts实例
      let myChart1 = this.$echarts.init(document.getElementById("myChart1"));
      // 绘制图表
      let option = {
        title: {
          text: "男女比例",
          x: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: "left",
          data: ["男", "女"]
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "60%"],
            data: [
              { value: 123, name: "男" },
              { value: 411, name: "女" }
            ],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            }
          }
        ]
      };
      myChart1.setOption(option);
    },
    drawLine2 () {
      // 基于准备好的dom，初始化echarts实例
      let myChart2 = this.$echarts.init(document.getElementById("myChart2"));
      // 绘制图表
      let option = {
        title: {
          text: "男女比例",
          x: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: "left",
          data: ["男", "女"]
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "60%"],
            data: [
              { value: 123, name: "男" },
              { value: 411, name: "女" }
            ],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            }
          }
        ]
      };
      myChart2.setOption(option);
    },
    drawLine3 () {
      // 基于准备好的dom，初始化echarts实例
      let myChart3 = this.$echarts.init(document.getElementById("myChart3"));
      // 绘制图表
      let option = {
        title: {
          text: "男女比例",
          x: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: "left",
          data: ["男", "女"]
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "60%"],
            data: [
              { value: 123, name: "男" },
              { value: 411, name: "女" }
            ],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            }
          }
        ]
      };
      myChart3.setOption(option);
    },
    drawLine4 () {
      // 基于准备好的dom，初始化echarts实例
      let myChart4 = this.$echarts.init(document.getElementById("myChart4"));
      // 绘制图表
      let option = {
        title: {
          text: "男女比例",
          x: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: "left",
          data: ["男", "女"]
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "60%"],
            data: [
              { value: 123, name: "男" },
              { value: 411, name: "女" }
            ],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            }
          }
        ]
      };
      myChart4.setOption(option);
    },
    drawLine5 () {
      // 基于准备好的dom，初始化echarts实例
      let myChart5 = this.$echarts.init(document.getElementById("myChart5"));
      // 绘制图表
      let option = {
        title: {
          text: "男女比例",
          x: "center"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          left: "left",
          data: ["男", "女"]
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            radius: "55%",
            center: ["50%", "60%"],
            data: [
              { value: 123, name: "男" },
              { value: 411, name: "女" }
            ],
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)"
              }
            }
          }
        ]
      };
      myChart5.setOption(option);
    }
  }
}
</script>

<style>
</style>
