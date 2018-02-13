<template>
  <div class="ratingselect">
    <div class="rating-type border-bottom-1px">
      <span class="block positive" @click='select(2,$event)' :class="{'active': selectType===2}">{{desc.all}}<span class='count'>{{ratings.length}}</span></span>
      <span class="block positive" @click='select(0,$event)' :class="{'active': selectType===0}">{{desc.positive}}<span class='count'>{{positives.length}}</span></span>
      <span class="block negative" @click='select(1,$event)' :class="{'active': selectType===1}">{{desc.negative}}<span class='count'>{{negatives.length}}</span></span>
    </div>
    <div class="switch border-bottom-1px" :class="{'on':onlyContent}" @click='toggleContent (onlyContent, $event)'>
      <span class="icon-check_circle"></span><span class="switch-item">只看有内容的评价</span>
    </div>
  </div>
</template>
<script>
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2
export default {
  data  () {
    return {
      selectType2: this.selectType,
      onlyContent2: this.onlyContent
    }
  },
  methods: {
    select (type, event) {
      if (!event._constructed) {
        return
      }
      this.$emit('rating-type', type)
      this.selectType2 = type
    },
    toggleContent (onlyContent, event) {
      if (!event._constructed) {
        return
      }
      this.$emit('only-content', onlyContent)
      this.onlyContent2 = !this.onlyContent2
    }
  },
  computed: {
    positives () {
      return this.ratings.filter((rating) => {
        return rating.rateType === POSITIVE
      })
    },
    negatives () {
      return this.ratings.filter((rating) => {
        return rating.rateType === NEGATIVE
      })
    }
  },
  props: {
    ratings: {
      type: Array,
      default () {
        return []
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    desc: {
      type: Object,
      default () {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  }
}
</script>

<style scoped lang='less'>
@import "../../common/less/mixins";
.ratingselect{
  .rating-type{
    margin: 0 0.96rem;
    padding: 0.96rem 0;
    font-size: 0;
    .border-bottom-1px(rgba(7,17,27,.1));
    .block{
      display: inline-block;
      padding:0.426667rem 0.64rem;
      margin-right: 0.426667rem;
      font-size: 0.64rem;
      color: rgb(77,85,93);
      line-height: 0.853333rem;
      border-radius: 0.053333rem;
      &.active{
        color: #fff;
      }
      .count{
        margin-left: 0.106667rem;
        font-size: 0.426667rem;
      }
      &.positive{
        background: rgba(0, 160, 220, .2);
        &.active {
            background: rgb(0, 160, 220);
        }
      }
      &.negative{
        background: rgba(77, 85, 93, .2);
        &.active {
            background: rgb(77, 85, 93);
        }
      }
    }
  }
  .switch{
    padding: 0.64rem 0.96rem;
    line-height: 1.28rem;
    .border-bottom-1px(rgba(7,17,27,.1));
    &.on{
     .icon-check_circle{
      color: #00c850;
     }
    }
    .icon-check_circle{
      display: inline-block;
      vertical-align: top;
      margin-right: 0.213333rem;
      font-size: 1.28rem;
      color: rgb(147,153,159);
    }
    .switch-item{
      display: inline-block;
      vertical-align: top;
      font-size: 0.64rem;
      color: rgb(147,153,159);
    }
  }
}
</style>
