<template>
  <div :class="className" :style="{height:height,width:width}"/>
</template>

<script>
import echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import { debounce } from '@/utils'

export default {
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '350px'
    },
    autoResize: {
      type: Boolean,
      default: true
    },
    chartData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      chart: null
    }
  },
  watch: {
    chartData: {
      deep: true,
      handler(val) {
        this.setOptions(val)
      }
    }
  },
  mounted() {
    this.initChart()
    if (this.autoResize) {
      this.__resizeHandler = debounce(() => {
        if (this.chart) {
          this.chart.resize()
        }
      }, 100)
      window.addEventListener('resize', this.__resizeHandler)
    }

    // 监听侧边栏的变化
    const sidebarElm = document.getElementsByClassName('sidebar-container')[0]
    sidebarElm.addEventListener('transitionend', this.sidebarResizeHandler)
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    if (this.autoResize) {
      window.removeEventListener('resize', this.__resizeHandler)
    }

    const sidebarElm = document.getElementsByClassName('sidebar-container')[0]
    sidebarElm.removeEventListener('transitionend', this.sidebarResizeHandler)

    this.chart.dispose()
    this.chart = null
  },
  methods: {
    sidebarResizeHandler(e) {
      if (e.propertyName === 'width') {
        this.__resizeHandler()
      }
    },
    setOptions({ chartNode, chartLink } = {}) {
      this.chart.setOption({
        tooltip: {
          show: true,
          showContent: true,
          trigger: 'item',
          formatter: function(x) {
            return x.data.name
          }
        },
        legend: {
          show: true,
          data: [{
            name: 'Main Developer',
            icon: 'rect'
          },
          {
            name: 'Owner',
            icon: 'rect'
          },
          {
            name: 'Contributor',
            icon: 'rect'
          },
          {
            name: 'Participant',
            icon: 'rect'
          }]
        },
        series: [{
          type: 'graph',
          name: 'Sentiment Relation',
          layout: 'force',
          legendHoverLink: true,
          force: {
            repulsion: 100, // 节点之间的斥力因子。支持数组表达斥力范围，值越大斥力越大。
            gravity: 0.03, // 节点受到的向中心的引力因子。该值越大节点越往中心点靠拢。
            edgeLength: 105, // 边的两个节点之间的距离，这个距离也会受 repulsion。[10, 50] 。值越小则长度越长
            layoutAnimation: true
          },
          roam: true,
          focusNodeAdjacency: true,
          symbolSize: 40,
          draggable: true,
          edgeSymbol: ['none', 'arrow'],
          itemStyle: {
            normal: {
              borderType: 'dashed', // 图形描边类型，默认为实线，支持 'solid'（实线）, 'dashed'(虚线), 'dotted'（点线）。
              borderWidth: 2, // 图形的描边线宽。为 0 时无描边。
              opacity: 1 // 图形透明度。支持从 0 到 1 的数字，为 0 时不绘制该图形。默认0.5
            }
          },
          label: {
            normal: {
              show: true,
              position: 'inside', // 标签的位置。['50%', '50%'] [x,y]
              textStyle: { // 标签的字体样式
                color: '#cde6c7', // 字体颜色
                fontStyle: 'normal', // 文字字体的风格 'normal'标准 'italic'斜体 'oblique' 倾斜
                fontWeight: 'bolder', // 'normal'标准'bold'粗的'bolder'更粗的'lighter'更细的或100 | 200 | 300 | 400...
                fontFamily: 'sans-serif', // 文字的字体系列
                fontSize: 12 // 字体大小
              }
            }
          },
          edgeLabel: {
            normal: {
              show: true,
              formatter: function(x) {
                return x.data.name
              }
            }
          },
          categories: [{
            name: 'Owner',
            symbol: 'rect'
          },
          {
            name: 'Contributor',
            symbol: 'rect'
          },
          {
            name: 'Participant',
            symbol: 'rect'
          },
          {
            name: 'Main Developer',
            symbol: 'rect'
          }],
          data: chartNode,
          links: chartLink
        }]
      })
    },
    initChart() {
      this.chart = echarts.init(this.$el, 'macarons')
      this.setOptions(this.chartData)
    }
  }
}
</script>
