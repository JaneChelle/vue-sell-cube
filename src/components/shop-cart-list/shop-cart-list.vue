<template>
  <transition name="fade">
      <cube-popup
      v-show="visible"
      :mask-closable=true
      :z-index=90
       position="bottom"
       type="shop-cart-list"
       @mask-click="maskClick">
          <transition name="move">
              <div v-show="visible">
                  <div class="list-header">
                    <h1 class="title">购物车</h1>
                    <span class="empty">清空</span>
                  </div>
                  <cube-scroll class="list-content" ref="listContent">
                    <ul>
                       <li class="food" v-for="food in selectFoods" :key="food.name">
                           <span class="name">{{food.name}}</span>
                           <div class="price">
                             <span>￥{{food.price*food.count}}</span>
                           </div>
                           <div class="cartcontrol-wrapper">
                             <cart-control  :food="food"></cart-control>
                           </div>
                       </li>
                    </ul>
                  </cube-scroll>
              </div>
          </transition>
      </cube-popup>
  </transition>
</template>

<script>
import CartControl from 'components/cart-control/cart-control'
export default {
    name: 'shop-cart-list',
    props: {
        selectFoods: {
            type: Array,
            default () {
              return []
            }
        }
    },
   data () {
    return {
        visible: false
    }
  },
  methods: {
      show () {
          this.visible = true
      },
      hide () {
          this.visible = false
      },
      maskClick () {
          this.hide()
      }
  },
  components: {
      CartControl
  }
}

</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import "~common/stylus/variable"
.cube-shop-cart-list
  bottom:48px
  &.fade-enter, &.fade-leave-active
    opacity: 0
  &.fade-enter-active, &.fade-leave-active
    transition: all .3s ease-in-out
  .move-enter, .move-leave-active
    transform: trans1ate3d(0,100%,0)
  .move-enter-active, .move-leave-active
    transition: al1 .3s ease-in-out
  .list-header
    height: 40px
    line-height: 40px
    padding: 0 18px
    background: $color-background-ssss
    .title
      float: left
      font-size: $fontsize-medium
      color: $color-dark-grey
    .empty
      float: right

</style>
