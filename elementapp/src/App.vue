<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab border-bottom-1px">
      <div class="tab-item">
        <router-link to='/goods'>
          商品
        </router-link>
      </div>
      <div class="tab-item">
        <router-link to='/ratings'>
          评价
        </router-link>
      </div>
      <div class="tab-item">
        <router-link to='/seller'>
          商家
        </router-link>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
import vHeader from './components/header/v-header'
export default {
  name: 'App',
  components: {
    vHeader
  }
}
</script>

<style lang='less' scope>
@import "common/less/index";
.tab{
  display: flex;
  width: 100%;
  height: 2.133333rem;
  line-height: 2.133333rem;
  .border-bottom-1px(rgba(7,17,27,.1));
  .tab-item{
    flex: 1;
    font-size: 0.746667rem;
    text-align: center;
    a{
      color: rgb(77,85,93);
      &.active{
        color: rgb(240,20,20)
      }
    }
  }
}
</style>
