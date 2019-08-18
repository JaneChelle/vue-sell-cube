<!--  -->
<template>
  <div class="tab">
      <cube-tab-bar v-model="selectedLabel" show-slider :data="tabs" ref="tabBar" :useTransition=false class="border-bottom-1px">
        <cube-tab v-for="(item) in tabs" :icon="item.icon" :label="item.label" :key="item.label">
        </cube-tab>
      </cube-tab-bar>
     <div class="slide-wrapper">
       <cube-slide ref="slide" :loop=false :auto-play=false :show-dots=false :initial-index="index" @change="onChange" @scroll="onScroll" :options="slideOptions">
        <cube-slide-item v-for="(tab, index) in tabs" :key="index">
         <components :is="tab.component" :data="tab.data" ref="component"></components>
        </cube-slide-item>
      </cube-slide>
     </div>
  </div>
</template>

<script>
export default {
  name: 'tab',
  props: {
   tabs: {
      type: Array,
      default () {
        return []
     }
    },
    initialIndex: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
        index: this.initialIndex,
        slideOptions: {
          listenScroll: true,
          probeType: 3,
          directionLockThreshold: 0
        }
    }
  },
  computed: {
    selectedLabel: {
      get () {
        return this.tabs[this.index].label
      },
      set (newVal) {
        this.index = this.tabs.findIndex((value) => {
            return value.label === newVal
        })
      }
    }
  },
  mounted () {
    this.onChange(this.index)
  },
  methods: {
    onChange (current) {
      this.index = current
      const component = this.$refs.component[current]
      component.fetch && component.fetch()
    },
    onScroll (pos) {
      const tabBarWidth = this.$refs.tabBar.$el.clientWidth
      const slideWidth = this.$refs.slide.slide.scrollerWidth
      const transform = -pos.x / slideWidth * tabBarWidth
      this.$refs.tabBar.setSliderTransform(transform)
    }
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus' scoped>
.tab
  >>>.cube-tab-bar
       align-items: baseline;
      .cube-tab
        padding: 10px 0
        display: flex
        flex-direction: column
        height: 100%
      .slide-wrapper
        flex: 1
        overflow: hidden
</style>
