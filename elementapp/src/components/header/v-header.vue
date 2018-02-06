<template>
  <div class="header">
    <div class="wrap">
        <div class="avatar">
            <img :src="seller.avatar">
        </div>
        <div class="content">
            <div class="title">
                <span class="brand"></span>
                <span class="name">{{seller.name}}</span>
            </div>
            <div class="description">
                {{seller.description}} / {{seller.deliveryTime}}分钟送达
            </div>
            <div class="supports" v-if='seller.supports'>
                <span class="icon" :class='mapSupports[seller.supports[0].type]'></span>
                <span class="text">{{seller.supports[0].description}}</span>
            </div>
        </div>
        <div class="supports-item" v-if='seller.supports' @click='detail'>
            <span>{{seller.supports.length}}个</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
    </div>
    <div class="bulletin">
        <span class="icon"></span><span class="bulletin-text">{{seller.bulletin}}</span>
        <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
        <img :src="seller.avatar" width='100%' height='100%'>
    </div>
    <transition name='fade'>
        <div class="detail" v-show='detailShow'>
            <div class="detail-wrapper">
                <div class="detail-main">
                    <h1>{{seller.name}}</h1>
                    <div class="star-wrapper">
                        <Star :size='48' :score='seller.score'></Star>
                    </div>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <ul v-if="seller.supports" class="detail-supports">
                        <li class="support-item" v-for="(item, index) in seller.supports" :key='index'>
                            <span class="icon" :class="mapSupports[seller.supports[index].type]"></span>
                            <span class="text">{{ seller.supports[index].description }}</span>
                        </li>
                    </ul>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">商家公告</div>
                        <div class="line"></div>
                    </div>
                    <div class="detail-bulletin">
                        {{seller.bulletin}}
                    </div>
                </div>
            </div>
            <div class="detail-close" @click='detail'>
                <i class="icon-close"></i>
            </div>
        </div>
    </transition>
  </div>
</template>

<script>
import Star from '@/components/star/star'
export default {
  data () {
    return {
      mapSupports: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
      detailShow: false
    }
  },
  methods: {
    detail () {
      this.detailShow = !this.detailShow
    }
  },
  props: ['seller'],
  components: {
    Star
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
@import "../../common/less/mixins";
.header{
    position: relative;
    background-color: rgba(7,17,27,.5);
    overflow: hidden;
    color: #fff;
    .wrap{
        position: relative;
        top: 0;
        left: 0;
        font-size: 0;
        padding: 1.28rem 0.64rem 0.96rem 1.28rem;
        .avatar{
            width: 3.413333rem;
            height: 3.413333rem;
            padding-right: 0.853333rem;
            display: inline-block;
            vertical-align: top;
            img{
                width: 3.413333rem;
                height: 3.413333rem;
                border-radius: 0.106667rem;
            }
        }
        .content{
            position: relative;
            display: inline-block;
            padding:0.106667rem 0;
            .title{
                margin-bottom: 0.426667rem;
                .brand{
                    display:inline-block;
                    width: 1.6rem;
                    height: 0.96rem;
                    .bg-image('brand');
                    background-size:1.6rem 0.96rem;
                    margin-right: 0.32rem;
                }
                .name{
                    font-size: 0.853333rem;
                    font-weight: bold;
                    height: 0.96rem;
                    vertical-align:top;
                }
            }
            .description{
                font-size: 0.533333rem;
                font-weight: 200;
                line-height: 0.64rem;
                margin-bottom: 0.533333rem;
            }
            .supports{
                .icon{
                    display: inline-block;
                    vertical-align: top;
                    width: 0.64rem;
                    height: 0.64rem;
                    background-size: 0.64rem 0.64rem;
                    background-repeat: no-repeat;
                    margin-right: 0.213333rem;
                    &.decrease{
                        .bg-image('decrease_1');
                    }
                    &.discount{
                        .bg-image('discount_1');
                    }
                    &.special{
                        .bg-image('special_1');
                    }
                    &.invoice{
                        .bg-image('invoice_1');
                    }
                    &.guarantee{
                        .bg-image('guarantee_1');
                    }
                }
                .text{
                    font-size: 0.533333rem;
                    font-weight: 200;
                    line-height: 0.64rem;
                }
            }
        }
        .supports-item{
            position: absolute;
            right: 0.64rem;
            bottom: 0.64rem;
            padding: 0.373333rem 0.426667rem;
            border-radius: 0.746667rem;
            background-color: rgba(0,0,0,.2);
            span{
                font-size: 0.533333rem;
                font-weight: 200;
                vertical-align: top;
            }
            i{
                font-size: 0.533333rem;
                margin-left: 0.106667rem;
                vertical-align: sub;
            }
        }
    }
    .bulletin{
        position: relative;
        height: 1.493333rem;
        line-height: 1.493333rem;
        padding: 0 1.173333rem 0 0.64rem;
        background: rgba(7, 17, 27, .2);
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        font-size: 0.533333rem;
        .icon{
            display: inline-block;
            vertical-align: top;
            margin-top: 0.346667rem;
            width: 1.173333rem;
            height: 0.64rem;
            .bg-image('bulletin');
            background-size: 1.173333rem 0.64rem;
            background-repeat: no-repeat;
        }
        .bulletin-text{
            margin: 0 0.213333rem;
            font-size: 0.533333rem;
            line-height: 1.493333rem;
        }
        .icon-keyboard_arrow_right{
            position: absolute;
            font-size: 0.533333rem;
            right: 0.64rem;
            top: 0.533333rem;
        }
    }
    .background{
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        z-index: -1;
        filter: blur(10px);
    }
    .detail{
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(7,17,27,.8);
        overflow: auto;
        z-index: 10;
        .detail-wrapper{
            width: 100%;
            min-height: 100%;
            .detail-main{
                padding-top: 3.413333rem;
                padding-bottom: 3.413333rem;
                h1{
                    font-size: 0.853333rem;
                    font-weight: 700;
                    line-height: 0.853333rem;
                    margin-bottom: 0.853333rem;
                    text-align: center;
                }
                .star-wrapper{
                    margin-top: 0.96rem;
                    padding: 0.106667rem 0;
                    text-align: center;
                }
                .title{
                    width: 80%;
                    margin: 0 auto;
                    display: flex;
                    margin: 1.493333rem auto 1.28rem auto;
                    .line{
                        flex:1;
                        position: relative;
                        top: -0.373333rem;
                        border-bottom: 1px solid rgba(255,255,255,.2);
                    }
                    .text{
                        font-size: 0.746667rem;
                        display: inline-block;
                        padding: 0 0.64rem;
                    }
                }
                .detail-supports {
                        width: 80%;
                        margin: 0 auto;
                        .support-item {
                            padding: 0 12px;
                            margin-bottom: 12px;
                            font-size: 0;
                            &:last-child {
                                margin-bottom: 0;
                            }
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
                            }
                        }
                    }
                .detail-bulletin{
                    width: 80%;
                    margin: 0 auto;
                    font-size: 0.64rem;
                    font-weight: 200;
                    line-height: 1.28rem;
                }
            }
        }
        .detail-close{
            position: relative;
            width: 1.706667rem;
            height: 1.706667rem;
            margin: -3.413333rem auto 0;
            clear: both;
            i{
                font-size: 1.706667rem;
                color: rgba(255,255,255,.5);
            }
        }
    }
    .fade-enter,.fade-leave-active{
        opacity: 0;
    }
    .fade-enter-active,.fade-leave-active{
        transition: all .5s
    }
}
</style>
