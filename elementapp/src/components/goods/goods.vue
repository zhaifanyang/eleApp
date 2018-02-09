<template>
  <div class="goods">
    <div class="menu-wrapper" ref='menuScroll'>
      <ul>
        <li v-for='(item,index) in goods' :key='index' class="menu-item" :class='{current:index===currentIndex}' @click='selectMenu(index,$event)'>
          <div class="list border-bottom-1px" >
            <span class="icon" v-show='item.type>0' :class='mapSupports[item.type]'></span><span class="name">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref='foodsScroll'>
      <ul>
        <li class="foods-item" v-for='(item,index) in goods' :key='index'>
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="content" v-for='(foods,index) in item.foods' :key='index'>
              <div class="icon">
                <img :src="foods.icon" class="icon-item">
              </div>
              <div class="desc">
                <h1 class="name">{{foods.name}}</h1>
                <div class="desc-item" v-if='foods.description'>{{foods.description}}</div>
                <div class="text">
                  <span class="month">月售{{foods.sellCount}}份</span><span>好评率{{foods.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{foods.price}}</span><span class="old" v-if='foods.oldPrice'>￥{{foods.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartControl :food='foods' v-on:cartAdd='cartAdd'></cartControl>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :deliveryPrice='seller.deliveryPrice' :minPrice='seller.minPrice' :selectFoods='selectFoods' ref='selectfood'></shopcart>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import shopcart from '@/components/shopcart/shopcart'
import cartControl from '@/components/cartControl/cartControl'
export default {
  data () {
    return {
      goods: [],
      mapSupports: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
      heightList: [],
      scrollY: 0
    }
  },
  methods: {
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuScroll, {
        // better-scroll 会将点击事件去掉，要在这里开启，同时点击在PC 会被执行两次，要在这里控制
        click: true
      })
      this.foodsScroll = new BScroll(this.$refs.foodsScroll, {
        click: true,
        probeType: 3
      })
      this.foodsScroll.on('scroll', pos => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _calculateHeight () {
      let height = 0
      let foods = this.$refs.foodsScroll.getElementsByClassName('foods-item')
      this.heightList.push(height)
      for (let i = 0; i < foods.length; i++) {
        height += foods[i].clientHeight
        this.heightList.push(height)
      }
    },
    selectMenu (index, event) {
      if (!event._constructed) {
        return
      }
      let foods = this.$refs.foodsScroll.getElementsByClassName('foods-item')
      this.foodsScroll.scrollToElement(foods[index], 400)
    },
    cartAdd (e) {
      this.$refs.selectfood.drop(e)
    }
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.heightList.length; i++) {
        let height1 = this.heightList[i]
        let height2 = this.heightList[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
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
    }
  },
  created () {
    this.$http.get('/api/goods').then(res => {
      this.goods = res.data.data
      this.$nextTick(() => {
        this._initScroll()
        this._calculateHeight()
      })
    }).catch(error => {
      console.log(error)
    })
  },
  props: ['seller'],
  components: {
    shopcart,
    cartControl
  }
}
</script>

<style scoped lang='less'>
@import "../../common/less/mixins";
.goods{
  display: flex;
  position: absolute;
  top: 9.28rem;
  bottom: 2.56rem;
  width: 100%;
  height: auto;
  overflow:hidden;
  .menu-wrapper{
    flex: 0 0 4.266667rem;
    width: 4.266667rem;
    font-size: 0.64rem;
    font-weight: 200;
    .menu-item{
      display: table;
      width: 4.266667rem;
      height: 2.88rem;
      background-color: #f3f5f7;
      padding: 0 0.64rem;
      box-sizing: border-box;
      &.current{
        position: relative;
        margin-top: -1px;
        background-color: #fff;
        z-index: 5;
        font-weight: 700;
        .list{
          .border-none;
        }
      }
      .list{
        width: 100%;
        height: 100%;
        vertical-align: middle;
        display: table-cell;
        .border-bottom-1px(rgba(7,17,27,.1));
        .name{
          line-height: 0.746667rem;
        }
        .icon{
          display: inline-block;
          vertical-align: top;
          margin-right: 0.053333rem;
          width: 0.64rem;
          height: 0.64rem;
          background-size: 0.64rem 0.64rem;
          background-repeat: no-repeat;
          &.decrease{
            .bg-image('decrease_3');
          }
          &.discount{
            .bg-image('discount_3');
          }
          &.special{
            .bg-image('special_3');
          }
          &.invoice{
            .bg-image('invoice_3');
          }
          &.guarantee{
            .bg-image('guarantee_3');
          }
        }
      }
    }
  }
  .foods-wrapper{
    flex: 1;
    .title{
      height: 1.386667rem;
      background-color: #f3f5f7;
      border-left: 1px solid #d9dde1;
      font-size: 0.64rem;
      color: rgb(147,153,159);
      line-height: 1.386667rem;
      padding-left: 0.746667rem;
    }
    .content{
      display: flex;
      margin: 0.96rem;
      padding-bottom: 0.96rem;
      .border-bottom-1px(rgba(7,17,27,.1));
      .icon{
        flex: 0 0 3.04rem;
        width: 3.04rem;
        height: 3.04rem;
        margin-right: 0.533333rem;
        .icon-item{
          width: 3.04rem;
          height: 3.04rem;
        }
      }
      .desc{
        flex: 1;
        font-size: 0.32rem;
        position: relative;
        .name{
          margin-top: 0.106667rem;
          line-height: 0.746667rem;
          color: rgb(7,17,27);
          font-size: 0.746667rem;
        }
        .desc-item{
          padding-top: 0.426667rem;
          font-size: 0.533333rem;
          color:rgb(147,153,159);
        }
        .text{
          padding-top: 0.426667rem;
          font-size: 0.533333rem;
          color:rgb(147,153,159);
          line-height: 0.533333rem;
          .month{
            padding-right: 0.64rem;
          }
        }
        .price{
          .now{
            line-height: 1.28rem;
            font-size: 0.746667rem;
            font-weight: 700;
            color: rgb(240,20,20);
            padding-right: 0.4rem;
          }
          .old{
            line-height: 1.28rem;
            font-size: 0.533333rem;
            font-weight: 700;
            color: rgb(147,153,159);
            text-decoration: line-through;
          }
        }
        .cartcontrol-wrapper{
          position: absolute;
          right: 0;
          bottom: -0.133333rem;
        }
      }
    }
  }
}
</style>
