<template>
  <div class="cartControl">
    <div class="cart-item">
      <transition name='fade'>
        <div class="del" v-show='food.count>0'  @click='cutCart()'>
          <span class="inner icon-remove_circle_outline"></span>
        </div>
      </transition>
      <div class="content" v-show='food.count>0'>{{food.count}}</div>
      <div class="add icon-add_circle" @click='addCart'></div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart (event) {
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      this.$emit('cartAdd', event.target)
    },
    cutCart () {
      this.food.count--
    }
  }
}
</script>

<style scoped lang='less'>
.cart-item{
  font-size: 0;
  .del,.add{
    display: inline-block;
    font-size: 1.28rem;
    line-height: 1.28rem;
    width: 1.28rem;
    height: 1.28rem;
    padding:0 0.64rem;
    color: rgb(0,160,220);
  }
  .content{
    display:inline-block;
    vertical-align: top;
    font-size: 0.533333rem;
    color: rgb(147,153,159);
    padding-top: 0.373333rem;
  }
}
.fade-enter-active,.fade-leave-active {
  transition: all .5s;
  transform: translate3d(0, 0, 0);
  .inner{
    transition: all .5s;
    transform: rotate(0deg);
  }
}
.fade-enter,.fade-leave-active {
  opacity: 0;
  transform: translate3d(1.28rem, 0, 0);
  .inner{
    transform: rotate(180deg);
  }
}
</style>
