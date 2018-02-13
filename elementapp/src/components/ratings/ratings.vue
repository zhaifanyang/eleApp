<template>
  <div class="ratings" ref='ratingSCroll'>
    <div class="rating-wrapper">
      <div class="rating-content">
        <div class="overview-content">
          <div class="overview-left border-right-1px">
            <h1 class="score">{{seller.serviceScore}}</h1>
            <div class="rating">综合评分</div>
            <div class="persent">高于周边商家{{seller.rankRate}}%</div>
          </div>
          <div class="overview-right">
            <div class="serviceScore">
              <span class="serviceScore-item">商家态度</span>
              <span class="star-wrapper">
                <Star :size='36' :score='seller.serviceScore'></Star>
              </span>
              <span class="score">{{seller.serviceScore}}</span>
            </div>
            <div class="foodScore">
              <span class="foodScore-item">产品分数</span>
              <span class="star-wrapper">
                <Star :size='36' :score='seller.foodScore'></Star>
              </span>
              <span class="score">{{seller.foodScore}}</span>
            </div>
            <div class="time-content">
              <span class="time">送达时间</span><span class="text">{{seller.deliveryTime}}分钟</span>
            </div>
          </div>
        </div>
        <splite></splite>
        <ratingselect :ratings='ratings' :selectType='selectType' :onlyContent='onlyContent' @rating-type='ratingType' @only-content='content'></ratingselect>
        <div class="rating-wrapper">
          <ul v-show='ratings.length'>
            <li v-show='needShow(rating.rateType,rating.text)' v-for="(rating,index) in ratings" class="rating-item border-bottom-1px" :key='index'>
              <img :src="rating.avatar" class="avatar">
              <div class="content">
                <p class="name">{{rating.username}}</p>
                <div class="time">{{rating.rateTime}}</div>
                <div class="star-wrapper">
                  <span class="star"><Star :size='24' :score='rating.score'></Star></span>
                  <span class="delivery" v-show='rating.deliveryTime'>{{rating.deliveryTim}}分钟送达</span>
                </div>
                <h1 class="text">{{rating.text}}</h1>
                <div class="rating-recommend" v-show='rating.recommend'>
                  <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
                  <span class="desc" v-for='(item,index) in rating.recommend' :key='index'>{{item}}</span>
                </div>
              </div>
            </li>
          </ul>
          <div class="no-rating" v-show="!ratings.length">暂无评价</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import splite from '@/components/splite/splite'
import Star from '@/components/star/star'
import ratingselect from '@/components/ratingselect/ratingselect'
import BScroll from 'better-scroll'
const ALL = 2
export default {
  data () {
    return {
      ratings: [],
      selectType: ALL,
      onlyContent: true
    }
  },
  components: {
    splite,
    Star,
    ratingselect
  },
  methods: {
    _initScroll () {
      this.scroll = new BScroll(this.$refs.ratingSCroll, {
        click: true
      })
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
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    }
  },
  created () {
    this.$http.get('/api/ratings').then(res => {
      this.ratings = res.data.data
      this.$nextTick(() => {
        this._initScroll()
      })
    }).catch(error => {
      console.log(error)
    })
  },
  props: ['seller']
}

</script>
<style scoped lang='less'>
@import "../../common/less/mixins";
.ratings {
  position: absolute;
  top: 9.28rem;
  bottom: 2.56rem;
  width: 100%;
  height: auto;
  overflow: hidden;
  .rating-wrapper {
    .rating-content {
      .overview-content {
        display: flex;
        margin: 0.96rem 0;
        .overview-left {
          flex: 0 0 7.333333rem;
          text-align: center;
          .border-right-1px(rgba(7,17,27,.1));
          .score{
            font-size: 1.28rem;
            color: rgb(255,153,0);
            line-height: 1.493333rem;
            padding-bottom: 0.32rem;
          }
          .rating{
            font-size: 0.64rem;
            color: rgb(7,17,27);
            line-height: 0.64rem;
            padding-bottom: 0.426667rem;
          }
          .persent{
            font-size: 0.533333rem;
            color: rgb(147,153,159);
            padding-bottom: 0.32rem;
          }
        }
        .overview-right {
          flex: 1;
          padding: 0 1.28rem;
          .serviceScore{
            font-size: 0;
            line-height: 0.96rem;
            .serviceScore-item{
              display: inline-block;
              vertical-align: top;
              font-size: 0.64rem;
              color: rgb(7,17,27);
            }
            .star-wrapper{
              display: inline-block;
              margin-top: 0.053333rem;
              padding: 0 0.266667rem;
            }
            .score{
              display: inline-block;
              vertical-align: top;
              font-size: 0.64rem;
              color: rgb(255,153,0);
            }
          }
          .foodScore{
            font-size: 0;
            line-height: 0.96rem;
            .foodScore-item{
              display: inline-block;
              vertical-align: top;
              font-size: 0.64rem;
              color: rgb(7,17,27);
            }
            .star-wrapper{
              display: inline-block;
              margin-top: 0.053333rem;
              padding: 0 0.266667rem;
            }
            .score{
              display: inline-block;
              vertical-align: top;
              font-size: 0.64rem;
              color: rgb(255,153,0);
            }
          }
          .time-content{
            line-height: 0.96rem;
            .time{
              display: inline-block;
              vertical-align: top;
              font-size: 0.64rem;
              color: rgb(7,17,27);
            }
            .text{
              display: inline-block;
              font-size: 0.64rem;
              color: rgb(147,153,159);
              vertical-align: top;
            }
          }
        }
      }
      .rating-wrapper{
        .rating-item{
          display: flex;
          margin: 0 0.96rem;
          padding: 0.96rem 0;
          .border-bottom-1px(rgba(7,17,27,.1));
          font-size: 0;
          .avatar{
            flex: 0 0 1.493333rem;
            display: inline-block;
            width: 1.493333rem;
            height: 1.493333rem;
          }
          .content{
            flex: 1;
            display: inline-block;
            position: relative;
            padding-left: 0.64rem;
            .name{
              display: inline-block;
              vertical-align: top;
              font-size: 0.533333rem;
              color: rgb(7,17,27);
              line-height: 0.64rem;
              margin-bottom: 0.213333rem;
            }
            .time{
              position: absolute;
              top: 0;
              right: 0;
              font-size: 0.533333rem;
              color: rgb(147,153,159);
              line-height: 0.64rem;
            }
            .star-wrapper{
              margin-bottom: 0.32rem;
              line-height: 0.64rem;
              .star{
                display: inline-block;
                margin-right: 0.16rem;
              }
              .delivery{
                display: inline-block;
                font-size: 0.533333rem;
                color: rgb(147,153,159);
                vertical-align: top;
                margin-top: 0.053333rem;
              }
            }
            .text{
              font-size: 0.64rem;
              color: rgb(7,17,27);
              line-height: 0.96rem;
            }
            .rating-recommend{
              margin-top: 0.426667rem;
              line-height: 0.853333rem;
              .icon-thumb_up,.icon-thumb_down{
                display: inline-block;
                color: rgb(0,160,220);
                font-size: 0.64rem;
                margin-right: 0.426667rem;
                vertical-align: top;
              }
              .icon-thumb_down{
                color: rgb(183,187,191);
              }
              .desc{
                display: inline-block;
                padding: 0 0.32rem;
                font-size: 0.48rem;
                color: rgb(147,153,159);
                margin-right: 0.426667rem;
                box-sizing: border-box;
                border: 1px solid rgba(7,17,27,.1);
              }
            }
          }
        }
      }
    }
  }
}

</style>
