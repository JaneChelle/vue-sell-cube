<template>
  <div class="goods">
    <div class="scroll-nav-wrapper">
      <cube-scroll-nav :side=true :data="goods" :options="scrollOptions" v-if="goods.length">
        <template slot="bar" slot-scope="props">
          <cube-scroll-nav-bar direction="vertical" :labels="props.labels" :txts="barTxts" :current="props.current">
            <template slot-scope="props">
              <div class="text">
                <support-ico v-if="props.txt.type>=1" :size=3 :type="props.txt.type"></support-ico>
                <span>{{props.txt.name}}</span>
                <span class="num" v-if="props.txt.count">
                  <bubble :num="props.txt.count"></bubble>
                </span>
              </div>
            </template>
          </cube-scroll-nav-bar>
        </template>
        <cube-scroll-nav-panel
          v-for="good in goods"
          :key="good.name"
          :label="good.name"
          :title="good.name">
          <ul>
            <li  @click="selectFood(food)"
                 v-for="food in good.foods"
                :key="food.name"
                class="food-item">
              <div class="icon">
                <img :src="food.icon" width="57" height="57">
              </div>
              <div class="content">
                 <h2 class="name">{{food.name}}</h2>
                 <p class="desc">{{food.description}}</p>
                 <div class="extra">
                    <span class="count">月销{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                 </div>
                 <div class="price">
                    <span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                 </div>
                  <div class="cart-control-wrapper">
                    <cart-control :food="food" @add="onAdd"></cart-control>
                  </div>
              </div>
            </li>
          </ul>
        </cube-scroll-nav-panel>
      </cube-scroll-nav>
    </div>
    <div class="shop-cart-wrapper">
        <shop-cart ref="shopCart" :delivery-price="seller.deliveryPrice"
         :min-price="seller.minPrice"
        :select-foods="selectFoods">
        </shop-cart>
    </div>
  </div>
</template>
<script>
import { getGoods } from 'api'
import ShopCart from 'components/shop-cart/shop-cart'
import CartControl from 'components/cart-control/cart-control'
import bubble from 'components/bubble/bubble'
import SupportIco from 'components/support-ico/support-ico'
export default {
  name: 'goods',
  components: {
      ShopCart,
      CartControl,
      bubble,
      SupportIco,
  },
  props: {
    data: {
      type: Object,
      default () {
        return {}
      }
    }
  },
  data () {
    return {
      goods: [],
      selectedFood: {},
      scrollOptions: {
        click: false,
        directionLockThreshold: 0
      }
    }
  },
  computed: {
    seller () {
      return this.data.seller
    },
      selectFoods () {
          let foods = []
          this.goods.forEach((good) => {
              good.foods.forEach((food) => {
                  if (food.count) {
                      foods.push(food)
                  }
              })
          })
          return foods
      },
      barTxts () {
          let ret = []
          this.goods.forEach((good) => {
              const { type, name, foods } = good
              let count = 0
              foods.forEach((food) => {
                  count += food.count || 0
              })
              ret.push({
                  type,
                  name,
                  count
              })
          })
          return ret
      }
  },
  methods: {
    fetch () {
        if (!this.fetched) {
          this.fetched = true
          getGoods().then((goods) => {
            this.goods = goods
          })
        }
    },
    onAdd (el) {
      this.$refs.shopCart.drop(el)
    },
    selectFood (food) {
        this.selectedFood = food
        this._showFood()
    },
    _showFood () {
        this.foodComp = this.foodComp || this.$createFood ({
            $props: {
                food: 'selectedFood'
            }
        })
        this.foodComp.show()
    }
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus' scoped>
@import '~common/stylus/mixin.styl'
@import "~common/stylus/variable"
  .goods
    text-align: left
    position: relative
    height: 100%
    .scroll-nav-wrapper
      position: absolute
      width: 100%
      top: 0
      left: 0
      bottom: 48px
    >>>.cube-scroll-nav-bar
        width: 80px
        white-space: normal
        overflow:hidden
      >>>.cube-sticky-fixed
          padding: 10px 0px
          background: #fff
      >>>.cube-scroll-nav-bar-item
          padding: 0 10px
          display: flex
          align-items: center
          height: 56px
          line-height: 14px
          font-size: $fontsize-small
          background: $color-background-ssss
          .text
            flex: 1
            position: relative
          .num
            position: absolute
            right: -8px
            top: -10px
          .support-ico
            display: inline-block
            vertical-align : top
            margin-right: 4px
      .food-item
        display: flex
        margin: 18px
        padding-bottom: 18px
        position: relative
        // border-1px(rgba(7,17,27,0.1))
        &:last-child
          border-none()
          margin-bottom: 0
        .icon
          flex: 0 0 57px
          margin-right: 10px
        .content
          flex: 1
          .name
            margin: 2px 0 8px 0
            height: 14px
            line-height:14px
            font-size: 14px
            color: rgb(7,17,27)
          .desc, .extra
            font-size: 10px
            line-height:10px
            color: rgb(147,153,159)
          .desc
            margin-bottom: 8px
            line-height: 12px
          .extra
            .count
              margin-right: 12px
          .price
            font-weight: 700
            line-height: 24px
            .now
              margin-right: 8px
              font-size: 14px
              color: rgb(240,20,20)
            .old
              text-decoration: line-through
              font-size:10px
              color: rgb(147,153,159)
          .cart-control-wrapper
            position: absolute
            right: 0
            bottom: 12px
        .shop-cart-wrapper
          position: absolute
          left: 0
          bottom: 0
          z-index: 50
          width: 100%
          height: 48px
</style>
