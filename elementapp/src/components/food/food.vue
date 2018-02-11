<template>
<transition name='move'>
  <div class="food" v-show='flag' ref='food'>
    <div class="food-content">
      <div class="food-header">
        <img :src="selectedFood.image" class="header-img">
        <div class="back" @click='back'>
          <i class="arrow icon-arrow_lift"></i>
        </div>
      </div>
      <div class="content">
        <div class="title">
          <h1 class="item">{{selectedFood.name}}</h1>
        </div>
        <div class="detail">
          <span class="sell-count">月售{{selectedFood.sellCount}}份</span><span class="rating">好评率{{selectedFood.rating}}%</span>
        </div>
        <div class="price">
          <span class="now">￥{{selectedFood.price}}</span><span class="old" v-if='selectedFood.oldPrice'>￥{{selectedFood.oldPrice}}</span>
        </div>
        <div class="cartControl-wrapper">
          <cartControl :food='selectedFood'></cartControl>
        </div>
        <transition name='fade'>
          <div class="buy" @click.stop.prevent='addFirst' v-show='!selectedFood.count||selectedFood.count===0'>加入购物车</div>
        </transition>
      </div>
    </div>
  </div>
</transition>
</template>
<script>
import Vue from 'Vue'
import BScroll from 'better-scroll'
import cartControl from '@/components/cartControl/cartControl'
export default {
  data () {
    return {
      flag: false
    }
  },
  methods: {
    show () {
      this.flag = true
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      })
    },
    back () {
      this.flag = false
    },
    addFirst (event) {
      if (!event._constructed) {
        return
      }
      this.$emit('addFirst', event.target)
      Vue.set(this.selectedFood, 'count', 1)
    }
  },
  props: {
    selectedFood: {
      type: Object
    }
  },
  components: {
    cartControl
  }
}

</script>
<style scoped lang='less'>
.food {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 2.56rem;
  width: 100%;
  background: #fff;
  z-index: 30;
  &.move-enter-active, &.move-leave-active {
    transition: all .2s linear;
  }
  &.move-enter, &.move-leave-active {
    transform: translate3d(100%, 0, 0);
  }
  .food-content{
    .food-header{
      position: relative;
      width: 100%;
      height: 0;
      padding-top: 100%;
      .header-img{
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
      }
      .back{
        position: absolute;
        top: 1.066667rem;
        left: 0.533333rem;
        width: 1.333333rem;
        height: 1.333333rem;
        .arrow{
          font-size: 1.066667rem;
          color: #fff;
          font-weight: 500;
        }
      }
    }
    .content{
      .title{
        padding: 0.96rem 0.96rem 0.426667rem 0.96rem;
        .item{
          font-size: 0.746667rem;
          font-weight: 700;
          color: rgb(7,17,27);
          line-height: 0.746667rem
        }
      }
      .detail{
        padding: 0 0.96rem 0.96rem 0.96rem;
        .sell-count,.rating{
          font-size: 0.533333rem;
          color: rgb(147,153,159);
          line-height: 0.533333rem;
        }
        .rating{
          padding-left: 0.64rem;
        }
      }
      .price{
        padding: 0 0.96rem 0.96rem 0.96rem;
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
      .cartControl-wrapper{
        position: absolute;
        right: 0.64rem;
        bottom: 0.64rem;
      }
      .buy{
        position: absolute;
        right: 0.96rem;
        bottom: 0.96rem;
        width: 3.946667rem;
        height: 1.28rem;
        border-radius: 0.64rem;
        font-size: 0.533333rem;
        color: rgb(255,255,255);
        line-height: 1.28rem;
        background-color: rgb(0,160,220);
        text-align: center;
        &.fade-enter-active{
          opacity: 1;
          transition: all .5s;
        }
        &.fade-leave-active{
          opacity: 0;
          transition: all .5s;
        }
        &.fade-enter{
          opacity: 0;
        }
        &.fade-leave{
          opacity: 1;
        }
      }
    }
  }
}

</style>
