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
        <div class="supports-item" v-if='seller.supports'>
            <span>{{seller.supports.length}}个</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      mapSupports: ['decrease_1', 'discount_1', 'special_1', 'invoice_1', 'guarantee_1']
    }
  },
  props: ['seller']
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
@import "../../common/less/mixins";
.header{
    background-color: rgba(7,17,27,.5);
    overflow: hidden;
    color: #fff;
    font-size: 0;
    .wrap{
        position: relative;
        top: 0;
        left: 0;
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
                    line-height: 0.96rem;
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
                    width: 0.64rem;
                    height: 0.64rem;
                    background-size: 0.64rem 0.64rem;
                    margin-right: 0.213333rem;
                    &.decrease_1{
                        .bg-image('decrease_1');
                    }
                    &.discount_1{
                        .bg-image('discount_1');
                    }
                    &.special_1{
                        .bg-image('special_1');
                    }
                    &.invoice_1{
                        .bg-image('invoice_1');
                    }
                    &.guarantee_1{
                        .bg-image('guarantee_1');
                    }
                }
                .text{
                    font-size: 0.533333rem;
                    font-weight: 200;
                    line-height: 0.64rem;
                    vertical-align: top;
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
            }
        }
    }
}
</style>
