<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for='(item,index) in goods' :key='index' class="menu-item">
          <div class="list">
            <span class="icon" v-if='item.type>0'></span><span class="name">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper">
      <div class="foods-item" v-for='(item,index) in goods' :key='index'>
        <div class="tilte">{{item.name}}</div>
        <div class="content" v-for='(foods,index) in item.foods' :key='index'>
          <div class="icon">
            <img :src="foods.icon">
          </div>
          <div class="desc">
            <h1 class="name">{{foods.name}}</h1>
            <div class="desc-item">{{foods.description}}</div>
            <div class="text">
              <span>月售{{foods.sellCount}}份</span><span>好评率{{foods.rating}}</span>
            </div>
            <div class="price">
              <span class="now">￥{{foods.price}}</span><span class="old" v-if='foods.oldPrice'>￥{{foods.oldPrice}}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      goods: [],
      mapSupports: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  },
  created () {
    this.$http.get('/api/goods').then(res => {
      this.goods = res.data.data
      console.log(this.goods)
    }).catch(error => {
      console.log(error)
    })
  },
  props: ['seller']
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
@import "../../common/less/mixins";
.goods{
  display: flex;
  position: absolute;
  top: 9.28rem;
  bottom: 2.56rem;
  width: 100%;
  height: auto;
  overflow: hidden;
  .menu-wrapper{
    flex: 0 0 4.266667rem;
    width: 4.266667rem;
    font-size: 0.64rem;
    font-weight: 200;
    .menu-item{
      width: 4.266667rem;
      height: 2.88rem;
      background-color: #f3f5f7;
      padding: 0 0.64rem;
      box-sizing: border-box;
      .list{
        width: 100%;
        height: 100%;
        display: table;
        .border-bottom-1px(rgba(7,17,27,.1));
        .name{
          line-height: 0.746667rem;
          vertical-align: middle;
          display: table-cell;
        }
      }
    }
  }
  .foods-wrapper{
    flex: 1;
  }
}
</style>
