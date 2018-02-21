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
      // ratings: [
      //   {
      //     'username': '3******c',
      //     'rateTime': 1469281964000,
      //     'deliveryTime': 30,
      //     'score': 5,
      //     'rateType': 0,
      //     'text': '不错,粥很好喝,我经常吃这一家,非常赞,以后也会常来吃,强烈推荐.',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': [
      //       '南瓜粥',
      //       '皮蛋瘦肉粥',
      //       '扁豆焖面',
      //       '娃娃菜炖豆腐',
      //       '牛肉馅饼'
      //     ]
      //   },
      //   {
      //     'username': '2******3',
      //     'rateTime': 1469271264000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '服务态度不错',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': [
      //       '扁豆焖面'
      //     ]
      //   },
      //   {
      //     'username': '3******b',
      //     'rateTime': 1469261964000,
      //     'score': 3,
      //     'rateType': 1,
      //     'text': '',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': '1******c',
      //     'rateTime': 1469261864000,
      //     'deliveryTime': 20,
      //     'score': 5,
      //     'rateType': 0,
      //     'text': '良心店铺',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': '2******d',
      //     'rateTime': 1469251264000,
      //     'deliveryTime': 10,
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': '9******0',
      //     'rateTime': 1469241964000,
      //     'deliveryTime': 70,
      //     'score': 1,
      //     'rateType': 1,
      //     'text': '送货速度蜗牛一样',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'd******c',
      //     'rateTime': 1469231964000,
      //     'deliveryTime': 30,
      //     'score': 5,
      //     'rateType': 0,
      //     'text': '很喜欢的粥店',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': '2******3',
      //     'rateTime': 1469221264000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '量给的还可以',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': '3******8',
      //     'rateTime': 1469211964000,
      //     'deliveryTime': '',
      //     'score': 3,
      //     'rateType': 1,
      //     'text': '',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'a******a',
      //     'rateTime': 1469201964000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '孩子喜欢吃这家',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': [
      //       '南瓜粥'
      //     ]
      //   },
      //   {
      //     'username': '3******3',
      //     'rateTime': 1469191264000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '粥挺好吃的',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 't******b',
      //     'rateTime': 1469181964000,
      //     'deliveryTime': '',
      //     'score': 3,
      //     'rateType': 1,
      //     'text': '',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'f******c',
      //     'rateTime': 1469171964000,
      //     'deliveryTime': 15,
      //     'score': 5,
      //     'rateType': 0,
      //     'text': '送货速度很快',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'k******3',
      //     'rateTime': 1469161264000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'u******b',
      //     'rateTime': 1469151964000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '下雨天给快递小哥点个赞',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 's******c',
      //     'rateTime': 1469141964000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '好',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'z******3',
      //     'rateTime': 1469131264000,
      //     'deliveryTime': '',
      //     'score': 5,
      //     'rateType': 0,
      //     'text': '吃了还想再吃',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'n******b',
      //     'rateTime': 1469121964000,
      //     'deliveryTime': '',
      //     'score': 3,
      //     'rateType': 1,
      //     'text': '发票开的不对',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'm******c',
      //     'rateTime': 1469111964000,
      //     'deliveryTime': 30,
      //     'score': 5,
      //     'rateType': 0,
      //     'text': '好吃',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'l******3',
      //     'rateTime': 1469101264000,
      //     'deliveryTime': 40,
      //     'score': 5,
      //     'rateType': 0,
      //     'text': '还不错吧',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': '3******o',
      //     'rateTime': 1469091964000,
      //     'deliveryTime': '',
      //     'score': 2,
      //     'rateType': 1,
      //     'text': '',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': '3******p',
      //     'rateTime': 1469081964000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '很喜欢的粥',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'o******k',
      //     'rateTime': 1469071264000,
      //     'deliveryTime': '',
      //     'score': 5,
      //     'rateType': 0,
      //     'text': '',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   },
      //   {
      //     'username': 'k******b',
      //     'rateTime': 1469061964000,
      //     'deliveryTime': '',
      //     'score': 4,
      //     'rateType': 0,
      //     'text': '',
      //     'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png',
      //     'recommend': []
      //   }
      // ],
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
  // mounted () {
  //   this._initScroll()
  // }
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
