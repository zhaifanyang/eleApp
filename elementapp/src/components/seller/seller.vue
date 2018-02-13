<template>
<div class="seller" ref='sellerScroll'>
  <div class="seller-wrapper">
    <div class="content">
      <div class="content-top">
        <h1 class="title">{{seller.name}}</h1>
        <div class="star-wrapper border-bottom-1px">
          <span class="star"><Star :score='seller.serviceScore' :size='36'></Star></span>
          <span class="ratingCount">({{seller.ratingCount}})</span>
          <span class="sellCount">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="seller-list">
          <li class="seller-item border-right-1px"><p class="item">起送价</p>{{seller.minPrice}}<span class="price">元</span></li>
          <li class="seller-item border-right-1px"><p class="item">商家配送</p>{{seller.deliveryPrice}}<span class="price">元</span></li>
          <li class="seller-item"><p class="item">平均配送时间</p>{{seller.deliveryPrice}}<span class="price">分钟</span></li>
        </ul>
        <div class="favorite">
          <span :class="{'active': collect}" class="icon-favorite" @click='toggle'></span>
          <span class="text">{{favorate}}</span>
        </div>
      </div>
      <splite></splite>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <p class="detail">{{seller.bulletin}}</p>
        <ul v-if="seller.supports" class="detail-supports">
            <li class="support-item border-top-1px" v-for="(item, index) in seller.supports" :key='index'>
                <span class="icon" :class="mapSupports[seller.supports[index].type]"></span>
                <span class="text">{{ seller.supports[index].description }}</span>
            </li>
        </ul>
      </div>
      <splite></splite>
      <div class="business">
        <h1 class="title">商家实景</h1>
        <div class="pic" ref='picScroll'>
          <ul ref='ul'>
            <li v-for='(pic,index) in seller.pics' :key='index' class="pic-item">
              <img :src="pic" width='120' height='80'>
            </li>
          </ul>
        </div>
      </div>
      <splite></splite>
      <div class="info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li v-for='(info,index) in seller.infos' :key="index" class="info-item border-top-1px">
            {{info}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import BScroll from 'better-scroll'
import Star from '@/components/star/star'
import splite from '@/components/splite/splite'
export default {
  data () {
    return {
      mapSupports: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
      collect: false
    }
  },
  computed: {
    favorate () {
      if (this.collect) {
        return '已收藏'
      } else {
        return '收藏'
      }
    }
  },
  methods: {
    toggle () {
      this.collect = !this.collect
    }
  },
  mounted () {
    this.sellerScroll = new BScroll(this.$refs.sellerScroll, {
      click: true
    })
    if (this.seller.pics) {
      let picWidth = 120
      let margin = 6
      let width = (picWidth + margin) * this.seller.pics.length - margin
      this.$refs.ul.style.width = width + 'px'
      this.$nextTick(() => {
        this.picScroll = new BScroll(this.$refs.picScroll, {
          scrollX: true,
          eventPassthrough: 'vertical'
        })
      })
    }
  },
  props: ['seller'],
  components: {
    Star,
    splite
  }
}
</script>

<style scoped lang='less'>
@import "../../common/less/mixins";
.seller{
  position: absolute;
  top: 9.28rem;
  bottom: 0;
  width: 100%;
  height: auto;
  overflow:hidden;
  .seller-wrapper{
    .content{
      padding-top: 0.96rem;
      font-size: 0;
      .content-top{
        position: relative;
        .title{
          margin: 0 0.96rem;
          font-size: 0.746667rem;
          color: rgb(7,17,27);
          line-height: 0.746667rem;
          margin-bottom: 0.426667rem;
        }
        .star-wrapper{
          margin: 0 0.96rem;
          padding-bottom: 0.96rem;
          .border-bottom-1px(rgba(7,17,27,.1));
          .star{
            display: inline-block;
            margin-right: 0.426667rem;
          }
          .ratingCount{
            display: inline-block;
            font-size: 0.533333rem;
            color: rgb(77, 85, 93);
            line-height: 0.96rem;
            vertical-align: top;
            margin-right: 0.64rem;
          }
          .sellCount{
            display: inline-block;
            font-size: 0.533333rem;
            color: rgb(77, 85, 93);
            line-height: 0.96rem;
            vertical-align: top;
          }
        }
        .seller-list{
          display: flex;
          padding-top: 0.96rem;
          overflow: hidden;
          white-space: nowrap;
          .seller-item{
            flex: 1;
            margin-bottom: 0.96rem;
            font-size: 1.28rem;
            color: rgb(7,17,27);
            line-height: 1.28rem;
            text-align: center;
            .border-right-1px(rgba(7,17,27,.1));
            &:last-child{
              .border-none;
            }
            .item{
              font-size: 0.533333rem;
              color: rgb(147,153,159);
              line-height: 0.533333rem;
              margin-bottom: 0.213333rem;
            }
            .price{
              font-size: 0.533333rem;
            }
          }
        }
        .favorite{
          position: absolute;
          right: 0.96rem;
          top: 0.96rem;
          width: 1.92rem;
          text-align: center;
          .icon-favorite{
            display: block;
            font-size: 1.28rem;
            line-height: 1.28rem;
            color: rgb(7,17,27);
            margin-bottom: 0.213333rem;
            &.active{
              color: rgb(240,20,20);
            }
          }
          .text{
            font-size: 0.533333rem;
            color: rgb(77,85,93);
            line-height: 0.533333rem;
          }
        }
      }
      .bulletin{
        padding-top: 0.96rem;
        .title{
          margin: 0 0.96rem;
          font-size: 0.746667rem;
          color: rgb(7,17,27);
          line-height: 0.746667rem;
          margin-bottom: 0.426667rem;
        }
        .detail{
          padding: 0 0.64rem 0.853333rem;
          font-size: 0.64rem;
          color: rgb(240,20,20);
          line-height: 1.28rem;
          margin: 0 0.96rem;
        }
        .detail-supports {
          .support-item {
            padding: 0.853333rem 0.64rem;
            margin: 0 0.96rem;
            font-size: 0;
            .border-top-1px(rgba(7,17,27,.1));
            .icon {
              display: inline-block;
              width: 0.853333rem;
              height: 0.853333rem;
              vertical-align: top;
              margin-right: 0.32rem;
              background-size: 0.853333rem 0.853333rem;
              background-repeat: no-repeat;
              &.decrease {
                  .bg-image('decrease_2');
              }
              &.discount {
                  .bg-image('discount_2');
              }
              &.guarantee {
                  .bg-image('guarantee_2');
              }
              &.invoice {
                  .bg-image('invoice_2');
              }
              &.special {
                  .bg-image('special_2');
              }
            }
            .text {
              font-size: 0.64rem;
              line-height: 0.853333rem;
              color: rgb(7,17,27)
            }
          }
        }
      }
      .business{
        padding: 0.96rem 0.96rem;
        .title{
          font-size: 0.746667rem;
          color: rgb(7,17,27);
          line-height: 0.746667rem;
          margin-bottom: 0.426667rem;
        }
        .pic{
          white-space: nowrap;
          overflow: hidden;
          .pic-item{
            display: inline-block;
            padding-right: 0.32rem;
          }
        }
      }
      .info{
        padding: 0.96rem 0.96rem 0;
        .title{
          font-size: 0.746667rem;
          color: rgb(7,17,27);
          line-height: 0.746667rem;
          margin-bottom: 0.426667rem;
        }
        .info-item{
          padding: 0.853333rem 0.64rem;
          font-size: 0.64rem;
          color: rgb(7,17,27);
          line-height: 0.853333rem;
          .border-top-1px(rgba(7,17,27,.1));
        }
      }
    }
  }
}
</style>
