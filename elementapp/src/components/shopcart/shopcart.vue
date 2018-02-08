<template>
<div>
  <div class="shopcart">
    <div class="content">
      <div class="icon-wrap">
        <div class="count" v-show='totalCount>0'>{{totalCount}}</div>
        <div class="ico" :class='{lineHeight: totalCount>0}'>
          <i class="icon-shopping_cart"></i>
        </div>
      </div>
      <div class="price">￥{{totalPrice}}</div>
      <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
    </div>
    <div class="shopping">
      <div class="pay" :class='{bgHeight: totalPrice>=minPrice}'>{{payDesc}}</div>
    </div>
  </div>
</div>

</template>

<script>
export default {
  data () {
    return {
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach(foods => {
        total += foods.price * foods.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach(foods => {
        count += foods.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送`
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice
        return `还差￥${diff}起送`
      } else {
        return '去支付'
      }
    }
  },
  props: {
    minPrice: {
      type: Number,
      default: 0
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    selectFoods: {
      type: Array,
      default () {
        return []
      }
    }
  }
}
</script>

<style scoped lang='less'>
.shopcart {
  display: flex;
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2.56rem;
  background-color: #141d27;
  .content{
    flex: 1;
    .icon-wrap{
      position:relative;
      left: 0.64rem;
      bottom: 0.426667rem;
      display: inline-block;
      width: 2.986667rem;
      height: 2.986667rem;
      border-radius: 50%;
      background-color: #141d27;
      box-sizing: border-box;
      padding: 0.32rem;
      .count{
        position: absolute;
        top: 0;
        right: 0;
        color: #fff;
        width: 1.28rem;
        height: 0.853333rem;
        line-height: 0.853333rem;
        text-align: center;
        background-color: #f01414;
        border-radius: 0.853333rem;
        font-size: 0.48rem;
        font-weight: 700;
        box-shadow: 0 4px 8px 0 rgba(0,0,0,.4);
      }
      .ico{
        width: 2.346667rem;
        height: 2.346667rem;
        line-height: 2.346667rem;
        border-radius: 50%;
        background-color: #2b343c;
        text-align: center;
        &.lineHeight{
          background: rgb(0, 160, 220);
          .icon-shopping_cart{
            color: #fff;
          }
        }
        .icon-shopping_cart{
          font-size: 1.28rem;
          color: #80858a;
          line-height: 2.346667rem;
        }
      }
    }
    .price{
      display: inline-block;
      position: relative;
      left: 0.96rem;
      bottom: 0.32rem;
      height: 1.28rem;
      padding-right: 0.64rem;
      line-height: 1.28rem;
      font-size: 0.853333rem;
      color: rgba(255,255,255,.4);
      font-weight: 700;
      border-right: 1px solid rgba(255,255,255,.1)
    }
    .desc{
      display: inline-block;
      font-size: 0.533333rem;
      color: rgba(255,255,255,.4);
      font-weight: 700;
      line-height: 1.28rem;
      vertical-align: top;
      margin: 0.8rem 0 0 1.28rem;
    }
  }
  .shopping {
    flex: 0 0 5.6rem;
    width: 5.6rem;
    .pay{
      font-size: 0.64rem;
      color: #979a9c;
      font-weight: 700;
      height: 2.56rem;
      line-height: 2.56rem;
      text-align: center;
      background-color: #2b333b;
      &.bgHeight{
        background-color: #00b43c;
        color: #fff;
      }
    }
  }
}
</style>
