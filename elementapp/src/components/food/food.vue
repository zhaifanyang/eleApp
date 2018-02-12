<template>
<transition name='move'>
  <div class="food" v-show='flag' ref='food'>
    <div class="food-content">
      <div class="food-header">
        <img :src="food.image" class="header-img">
        <div class="back" @click='back'>
          <i class="arrow icon-arrow_lift"></i>
        </div>
      </div>
      <div class="content">
        <div class="title">
          <h1 class="item">{{food.name}}</h1>
        </div>
        <div class="detail">
          <span class="sell-count">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}%</span>
        </div>
        <div class="price">
          <span class="now">￥{{food.price}}</span><span class="old" v-if='food.oldPrice'>￥{{food.oldPrice}}</span>
        </div>
        <div class="cartControl-wrapper">
          <cartControl :food='food' @cartAdd='cartAdd' ref='cartcontrol'></cartControl>
        </div>
        <transition name='fade'>
          <div class="buy" @click.stop.prevent='addFirst' v-show="!food.count || food.count === 0">加入购物车</div>
        </transition>
      </div>
      <splite v-show='food.info'></splite>
      <div class="info" v-show='food.info'>
        <h1 class="title">商品介绍</h1>
        <p class="text">{{food.info}}</p>
      </div>
      <splite v-show='food.info'></splite>
      <div class="ratings">
        <h1 class="title">商品评价</h1>
        <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings" @rating-type='ratingType' @only-content='content'></ratingselect>
        <div class="rating-wrapper">
          <ul v-show='food.ratings&&food.ratings.length'>
            <li v-show='needShow(rating.rateType,rating.text)' v-for="(rating,index) in food.ratings" class="rating-item border-bottom-1px" :key='index'>
              <div class="user">
                <span class="name">{{rating.username}}</span>
                <img :src="rating.avatar" class="avatar" width="12" height="12">
              </div>
              <div class="time">{{rating.rateTime | formatDate}}</div>
              <div class="text">
                <span :class="{'icon-thumb_up': rating.rateType === 0, 'icon-thumb_down': rating.rateType === 1}"></span>{{rating.text}}
              </div>
            </li>
          </ul>
          <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
        </div>
      </div>
    </div>
  </div>
</transition>
</template>
<script>
import BScroll from 'better-scroll'
import cartControl from '@/components/cartControl/cartControl'
import splite from '@/components/splite/splite'
import ratingselect from '@/components/ratingselect/ratingselect'
import {formatDate} from '@/common/js/date'
// const POSITIVE = 0
// const NEGATIVE = 1
const ALL = 2
export default {
  data () {
    return {
      flag: false,
      selectType: ALL,
      onlyContent: true,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  methods: {
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    },
    content (onlyContent) {
      this.onlyContent = !this.onlyContent
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    ratingType (type) {
      this.selectType = type
      this.$nextTick(() => {
        this.scroll.refresh()
      })
    },
    cartAdd (target) {
      this.$emit('cartAdd', target)
    },
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
      this.$refs.cartcontrol.addCart(event)
    }
  },
  props: {
    food: {
      type: Object
    }
  },
  components: {
    cartControl,
    splite,
    ratingselect
  },
  filters: {
    formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  }
}

</script>
<style scoped lang='less'>
@import "../../common/less/mixins";
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
      position: relative;
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
    .info{
      padding: 0.96rem;
      .title{
        padding-bottom: 0.32rem;
        font-size: 0.746667rem;
        font-weight: 700;
        color: rgb(7,17,27);
        line-height: 0.746667rem;
      }
      .text{
        font-size: 0.64rem;
        font-weight: 200;
        color: rbg(147,153,159);
        line-height: 1.28rem;
      }
    }
    .ratings{
      padding-top: 0.48rem;
      .title{
        margin-left: 18px;
        font-size: 0.746667rem;
        font-weight: 700;
        color: rgb(7,17,27);
        line-height: 0.746667rem
      }
      .rating-wrapper{
        .rating-item{
          position: relative;
          padding: 0.853333rem 0;
          margin: 0 0.96rem;
          .border-bottom-1px(rgba(7,17,27,.1));
          .user{
            position: absolute;
            right: 0;
            top: 0.853333rem;
            margin-bottom: 0.32rem;
            line-height: 0.64rem;
            .name{
              font-size: 0.533333rem;
              color: rgb(147,153,159);
              vertical-align: top;
            }
            .avatar{
              margin-left: 0.32rem;
              width: 0.64rem;
              height: 0.64rem;
            }
          }
          .time{
            margin-bottom: 0.32rem;
            font-size: 0.533333rem;
            color: rgb(147,153,159);
            line-height: 0.64rem;
          }
          .text{
            font-size: 0.64rem;
            color: rgb(7,17,27);
            line-height: 0.853333rem;
            .icon-thumb_up,.icon-thumb_down{
              font-size: 0.64rem;
              color: rgb(0,160,220);
              line-height: 1.28rem;
              margin-right: 0.213333rem;
            }
            .icon-thumb_down{
              color: rgb(147,153,159);
            }
          }
        }
        .no-rating {
          padding: 16px;
          font-size: 12px;
          color: rgb(147, 153, 159);
        }
      }
    }
  }
}

</style>
