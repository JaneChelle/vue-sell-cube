<!--  -->
<template>
  <div class="header" @click="showDetail">
      <div class="content-wrapper">
        <div class="avatar">
          <img :src="seller.avatar" alt="" width="64" height="64">
        </div>
        <div class="content">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
          <div v-if="seller.supports" class="support">
           <support-ico :size="1" :type="seller.supports[0].type"></support-ico>
           <span class="text">{{seller.supports[0].description}}</span>
          </div>
        </div>
        <div v-if="seller.supports" class="support-count">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
      </div>
      <div class="bulletin-wrapper" >
        <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
         <i class="icon-keyboard_arrow_right"></i>
      </div>
      <div class="background">
        <img :src="seller.avatar" alt="" width="100%" height="100%">
      </div>
  </div>
</template>

<script>
import SupportIco from 'components/support-ico/support-ico'
export default {
  name: 'v-header',
  props: {
    seller: {
      type: Object,
      default () {
          return {}
      }
    }
  },
  methods: {
    showDetail () {
      this.HeaderDetailComp = this.HeaderDetailComp || this.$createHeaderDetail({
        $props: {
          seller: 'seller'
        }
      })
      this.HeaderDetailComp.show()
    }
  },
  components: {
    SupportIco
  }
}
</script>

<style lang='stylus' rel="stylesheet/stylus" scoped>
@import "~common/stylus/mixin";
// @import "~common/stylus/variable"
  .header
    color :#fff
    background-color :rgba(7,17,27,0.5)
    position :relative
    overflow :hidden
    .content-wrapper
      position: relative
      padding :24px 12px 18px 24px
      font-size :0
      .avatar
        display :inline-block
        vertical-align :top
        img
         border-radius :2px
      .content
         display :inline-block
         margin-left :16px
         .title
           margin : 2px 0 8px 0
           .brand
             display inline-block
             vertical-align :top
             width : 30px
             height 18px
             bg-image('brand')
             background-size 30px 18px
             background-repeat :no-repeat
           .name
             margin-left :6px
             font-size :16px
             line-height 18px
             font-weight :bold
         .description
           margin-bottom :10px
           line-height :12px
           font-size :12px
         .support
          .text
            line-height:12px
            font-size: 10px
      .support-count
        position: absolute
        right: 12px
        bottom: 14px
        height: 24px
        line-height: 24px
        border-radius: 14px
        background: rgba(0,0,0,.2)
        padding :4px 8px
        text-align: center
        .count
          vertical-align:top
          font-size: 10px
        .icon-keyboard_arrow_right
          margin-left :2px
          line-height : 24px
          font-size: 10px
    .bulletin-wrapper
      position: relative
      height: 28px
      line-height: 28px
      padding:0 22px 0 12px
      white-space:nowrap
      overflow : hidden
      text-overflow: ellipsis
      .bulletin-title
        display :inline-block
        vertical-align :top
        margin-top :8px
        width :22px
        height:12px
        bg-image('bulletin')
        background-size :22px 12px
        background-repeat :no-repeat
       .bulletin-text
         vertical-align :top
         margin:0 4px
         font-size :10px
       .icon-keyboard_arrow_right
         position: absolute
         font-size :10px
         right :12px
         top: 10px
    .background
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: -1
      filter: blur(10px)
</style>
