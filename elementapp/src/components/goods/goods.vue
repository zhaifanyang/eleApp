<template>
  <div class="goods">
    <div class="menu-wrapper" ref='menuScroll'>
      <ul>
        <li v-for='(item,index) in goods' :key='index' class="menu-item" :class='{current:index===currentIndex}' @click='selectMenu(index,$event)'>
          <div class="list border-bottom-1px">
            <span class="icon" v-show='item.type>0' :class='mapSupports[item.type]'></span><span class="name">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref='foodsScroll'>
      <ul>
        <li class="foods-item" v-for='(item,index) in goods' :key='index'>
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="content" v-for='(foods,index) in item.foods' :key='index' @click='selectFood(foods,$event)'>
              <div class="icon">
                <img :src="foods.icon" class="icon-item">
              </div>
              <div class="desc">
                <h1 class="name">{{foods.name}}</h1>
                <div class="desc-item" v-if='foods.description'>{{foods.description}}</div>
                <div class="text">
                  <span class="month">月售{{foods.sellCount}}份</span><span>好评率{{foods.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{foods.price}}</span><span class="old" v-if='foods.oldPrice'>￥{{foods.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartControl :food='foods' @cartAdd='cartAdd'></cartControl>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :deliveryPrice='seller.deliveryPrice' :minPrice='seller.minPrice' :selectFoods='selectFoods' ref='selectfood'></shopcart>
    <food :food='selectedFood' ref='food' @cartAdd='cartAdd'></food>
  </div>
</template>
<script>
import BScroll from 'better-scroll'
import shopcart from '@/components/shopcart/shopcart'
import cartControl from '@/components/cartControl/cartControl'
import food from '@/components/food/food'
export default {
  data () {
    return {
      // goods: [{
      //   'name': '热销榜',
      //   'type': -1,
      //   'foods': [{
      //     'name': '皮蛋瘦肉粥',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '咸粥',
      //     'sellCount': 229,
      //     'rating': 100,
      //     'info': '一碗皮蛋瘦肉粥，总是我到粥店时的不二之选。香浓软滑，饱腹暖心，皮蛋的Q弹与瘦肉的滑嫩伴着粥香溢于满口，让人喝这样的一碗粥也觉得心满意足',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '很喜欢的粥',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 1,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '扁豆焖面',
      //     'price': 14,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 188,
      //     'rating': 96,
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 1,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'info': '',
      //     'icon': 'http://fuss10.elemecdn.com/c/6b/29e3d29b0db63d36f7c500bca31d8jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/c/6b/29e3d29b0db63d36f7c500bca31d8jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '葱花饼',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 124,
      //     'rating': 85,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 1,
      //       'text': '没啥味道',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 1,
      //       'text': '很一般啊',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/f/28/a51e7b18751bcdf871648a23fd3b4jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/f/28/a51e7b18751bcdf871648a23fd3b4jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '牛肉馅饼',
      //     'price': 14,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 114,
      //     'rating': 91,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 1,
      //       'text': '难吃不推荐',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/d/b9/bcab0e8ad97758e65ae5a62b2664ejpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/d/b9/bcab0e8ad97758e65ae5a62b2664ejpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '招牌猪肉白菜锅贴/10个',
      //     'price': 17,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 101,
      //     'rating': 78,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 1,
      //       'text': '不脆,不好吃',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/7/72/9a580c1462ca1e4d3c07e112bc035jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/7/72/9a580c1462ca1e4d3c07e112bc035jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '南瓜粥',
      //     'price': 9,
      //     'oldPrice': '',
      //     'description': '甜粥',
      //     'sellCount': 91,
      //     'rating': 100,
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/8/a6/453f65f16b1391942af11511b7a90jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/8/a6/453f65f16b1391942af11511b7a90jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '红豆薏米美肤粥',
      //     'price': 12,
      //     'oldPrice': '',
      //     'description': '甜粥',
      //     'sellCount': 86,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/d/22/260bd78ee6ac6051136c5447fe307jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/d/22/260bd78ee6ac6051136c5447fe307jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '八宝酱菜',
      //     'price': 4,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 84,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '红枣山药糙米粥',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 81,
      //     'rating': 91,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '糊塌子',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 80,
      //     'rating': 93,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/0/05/097a2a59fd2a2292d08067e16380cjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/0/05/097a2a59fd2a2292d08067e16380cjpeg.jpeg?imageView2/1/w/750/h/750'
      //   }
      //   ]
      // },
      // {
      //   'name': '单人精彩套餐',
      //   'type': 2,
      //   'foods': [{
      //     'name': '红枣山药粥套餐',
      //     'price': 29,
      //     'oldPrice': 36,
      //     'description': '红枣山药糙米粥,素材包,爽口莴笋丝,四川泡菜或八宝酱菜,配菜可备注',
      //     'sellCount': 17,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }],
      //     'icon': 'http://fuss10.elemecdn.com/6/72/cb844f0bb60c502c6d5c05e0bddf5jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/6/72/cb844f0bb60c502c6d5c05e0bddf5jpeg.jpeg?imageView2/1/w/750/h/750'
      //   }]
      // },
      // {
      //   'name': '冰爽饮品限时特惠',
      //   'type': 1,
      //   'foods': [{
      //     'name': 'VC无限橙果汁',
      //     'price': 8,
      //     'oldPrice': 10,
      //     'description': '',
      //     'sellCount': 15,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '还可以',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/e/c6/f348e811772016ae24e968238bcbfjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/e/c6/f348e811772016ae24e968238bcbfjpeg.jpeg?imageView2/1/w/750/h/750'
      //   }]
      // },
      // {
      //   'name': '精选热菜',
      //   'type': -1,
      //   'foods': [{
      //     'name': '娃娃菜炖豆腐',
      //     'price': 17,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 43,
      //     'rating': 92,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '菜量还可以,味道还可以',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/d/2d/b1eb45b305635d9dd04ddf157165fjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/d/2d/b1eb45b305635d9dd04ddf157165fjpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '手撕包菜',
      //     'price': 16,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 29,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/9/c6/f3bc84468820121112e79583c24efjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/9/c6/f3bc84468820121112e79583c24efjpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '香酥黄金鱼/3条',
      //     'price': 11,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 15,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/4/e7/8277a6a2ea0a2e97710290499fc41jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/4/e7/8277a6a2ea0a2e97710290499fc41jpeg.jpeg?imageView2/1/w/750/h/750'
      //   }
      //   ]
      // },
      // {
      //   'name': '爽口凉菜',
      //   'type': -1,
      //   'foods': [{
      //     'name': '八宝酱菜',
      //     'price': 4,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 84,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '拍黄瓜',
      //     'price': 9,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 28,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/6/54/f654985b4e185f06eb07f8fa2b2e8jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/6/54/f654985b4e185f06eb07f8fa2b2e8jpeg.jpeg?imageView2/1/w/750/h/750'
      //   }
      //   ]
      // },
      // {
      //   'name': '精选套餐',
      //   'type': -1,
      //   'foods': [{
      //     'name': '红豆薏米粥套餐',
      //     'price': 37,
      //     'oldPrice': '',
      //     'description': '红豆薏米粥,三鲜干蒸烧卖,拍黄瓜',
      //     'sellCount': 3,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }],
      //     'icon': 'http://fuss10.elemecdn.com/f/49/27f26ed00c025b2200a9ccbb7e67ejpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/f/49/27f26ed00c025b2200a9ccbb7e67ejpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '皮蛋瘦肉粥套餐',
      //     'price': 31,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 12,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }],
      //     'icon': 'http://fuss10.elemecdn.com/8/96/f444a8087f0e940ef264617f9d98ajpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/8/96/f444a8087f0e940ef264617f9d98ajpeg.jpeg?imageView2/1/w/750/h/750'
      //   }
      //   ]
      // },
      // {
      //   'name': '果拼果汁',
      //   'type': -1,
      //   'foods': [{
      //     'name': '蜜瓜圣女萝莉杯',
      //     'price': 6,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 1,
      //     'rating': '',
      //     'info': '',
      //     'ratings': [],
      //     'icon': 'http://fuss10.elemecdn.com/b/5f/b3b04c259d5ec9fa52e1856ee50dajpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/b/5f/b3b04c259d5ec9fa52e1856ee50dajpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '加多宝',
      //     'price': 6,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 7,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/b/9f/5e6c99c593cf65229225c5661bcdejpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/b/9f/5e6c99c593cf65229225c5661bcdejpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': 'VC无限橙果汁',
      //     'price': 8,
      //     'oldPrice': 10,
      //     'description': '',
      //     'sellCount': 15,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '还可以',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/e/c6/f348e811772016ae24e968238bcbfjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/e/c6/f348e811772016ae24e968238bcbfjpeg.jpeg?imageView2/1/w/750/h/750'
      //   }
      //   ]
      // },
      // {
      //   'name': '小吃主食',
      //   'type': -1,
      //   'foods': [{
      //     'name': '扁豆焖面',
      //     'price': 14,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 188,
      //     'rating': 96,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 1,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/c/6b/29e3d29b0db63d36f7c500bca31d8jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/c/6b/29e3d29b0db63d36f7c500bca31d8jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '葱花饼',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 124,
      //     'rating': 85,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 1,
      //       'text': '没啥味道',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 1,
      //       'text': '很一般啊',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/f/28/a51e7b18751bcdf871648a23fd3b4jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/f/28/a51e7b18751bcdf871648a23fd3b4jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '牛肉馅饼',
      //     'price': 14,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 114,
      //     'rating': 91,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 1,
      //       'text': '难吃不推荐',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/d/b9/bcab0e8ad97758e65ae5a62b2664ejpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/d/b9/bcab0e8ad97758e65ae5a62b2664ejpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '招牌猪肉白菜锅贴/10个',
      //     'price': 17,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 101,
      //     'rating': 78,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 1,
      //       'text': '不脆,不好吃',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/7/72/9a580c1462ca1e4d3c07e112bc035jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/7/72/9a580c1462ca1e4d3c07e112bc035jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '糊塌子',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 80,
      //     'rating': 93,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/0/05/097a2a59fd2a2292d08067e16380cjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/0/05/097a2a59fd2a2292d08067e16380cjpeg.jpeg?imageView2/1/w/750/h/750'
      //   }
      //   ]
      // },
      // {
      //   'name': '特色粥品',
      //   'type': -1,
      //   'foods': [{
      //     'name': '皮蛋瘦肉粥',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '咸粥',
      //     'sellCount': 229,
      //     'rating': 100,
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '很喜欢的粥',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 1,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/c/cd/c12745ed8a5171e13b427dbc39401jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '南瓜粥',
      //     'price': 9,
      //     'oldPrice': '',
      //     'description': '甜粥',
      //     'sellCount': 91,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/8/a6/453f65f16b1391942af11511b7a90jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/8/a6/453f65f16b1391942af11511b7a90jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '红豆薏米美肤粥',
      //     'price': 12,
      //     'oldPrice': '',
      //     'description': '甜粥',
      //     'sellCount': 86,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/d/22/260bd78ee6ac6051136c5447fe307jpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/d/22/260bd78ee6ac6051136c5447fe307jpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '红枣山药糙米粥',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '',
      //     'sellCount': 81,
      //     'rating': 91,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/9/b5/469d8854f9a3a03797933fd01398bjpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '鲜蔬菌菇粥',
      //     'price': 11,
      //     'oldPrice': '',
      //     'description': '咸粥',
      //     'sellCount': 56,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': ''
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/e/a3/5317c68dd618929b6ac05804e429ajpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/e/a3/5317c68dd618929b6ac05804e429ajpeg.jpeg?imageView2/1/w/750/h/750'
      //   },
      //   {
      //     'name': '田园蔬菜粥',
      //     'price': 10,
      //     'oldPrice': '',
      //     'description': '咸粥',
      //     'sellCount': 33,
      //     'rating': 100,
      //     'info': '',
      //     'ratings': [{
      //       'username': '3******c',
      //       'rateTime': 1469281964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '2******3',
      //       'rateTime': 1469271264000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     },
      //     {
      //       'username': '3******b',
      //       'rateTime': 1469261964000,
      //       'rateType': 0,
      //       'text': '',
      //       'avatar': 'http://static.galileo.xiaojukeji.com/static/tms/default_header.png'
      //     }
      //     ],
      //     'icon': 'http://fuss10.elemecdn.com/a/94/7371083792c19df00e546b29e344cjpeg.jpeg?imageView2/1/w/114/h/114',
      //     'image': 'http://fuss10.elemecdn.com/a/94/7371083792c19df00e546b29e344cjpeg.jpeg?imageView2/1/w/750/h/750'
      //   }
      //   ]
      // }
      // ],
      goods: [],
      mapSupports: ['decrease', 'discount', 'special', 'invoice', 'guarantee'],
      heightList: [],
      scrollY: 0,
      selectedFood: {}
    }
  },
  methods: {
    selectFood (foods, event) {
      if (!event._constructed) {
        return
      }
      this.selectedFood = foods
      this.$refs.food.show()
    },
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuScroll, {
        // better-scroll 会将点击事件去掉，要在这里开启，同时点击在PC 会被执行两次，要在这里控制
        click: true
      })
      this.foodsScroll = new BScroll(this.$refs.foodsScroll, {
        click: true,
        probeType: 3
      })
      this.foodsScroll.on('scroll', pos => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _calculateHeight () {
      let height = 0
      let foods = this.$refs.foodsScroll.getElementsByClassName('foods-item')
      this.heightList.push(height)
      for (let i = 0; i < foods.length; i++) {
        height += foods[i].clientHeight
        this.heightList.push(height)
      }
    },
    selectMenu (index, event) {
      if (!event._constructed) {
        return
      }
      let foods = this.$refs.foodsScroll.getElementsByClassName('foods-item')
      this.foodsScroll.scrollToElement(foods[index], 400)
    },
    // cartAdd (e) {
    //   this.$refs.selectfood.drop(e)
    // },
    cartAdd (e) {
      // 体验优化，异步执行下落动画
      this.$nextTick(() => {
        // 父组件传递给子组件
        this.$refs.selectfood.drop(e)
      })
    }
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.heightList.length; i++) {
        let height1 = this.heightList[i]
        let height2 = this.heightList[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    },
    selectFoods () {
      let foods = []
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food)
          }
        })
      })
      return foods
    }
  },
  created () {
    this.$http.get('/api/goods').then(res => {
      this.goods = res.data.data
      this.$nextTick(() => {
        this._initScroll()
        this._calculateHeight()
      })
    }).catch(error => {
      console.log(error)
    })
  },
  props: ['seller'],
  components: {
    shopcart,
    cartControl,
    food
  }
  // mounted () {
  //   this._initScroll()
  //   this._calculateHeight()
  // }
}

</script>
<style scoped lang='less'>
@import "../../common/less/mixins";
.goods {
  display: flex;
  position: absolute;
  top: 9.28rem;
  bottom: 2.56rem;
  width: 100%;
  height: auto;
  overflow: hidden;
  .menu-wrapper {
    flex: 0 0 4.266667rem;
    width: 4.266667rem;
    font-size: 0.64rem;
    font-weight: 200;
    .menu-item {
      display: table;
      width: 4.266667rem;
      height: 2.88rem;
      background-color: #f3f5f7;
      padding: 0 0.64rem;
      box-sizing: border-box;
      &.current {
        position: relative;
        margin-top: -1px;
        background-color: #fff;
        z-index: 5;
        font-weight: 700;
        .list {
          .border-none;
        }
      }
      .list {
        width: 100%;
        height: 100%;
        vertical-align: middle;
        display: table-cell;
        .border-bottom-1px(rgba(7, 17, 27, .1));
        .name {
          line-height: 0.746667rem;
        }
        .icon {
          display: inline-block;
          vertical-align: top;
          margin-right: 0.053333rem;
          width: 0.64rem;
          height: 0.64rem;
          background-size: 0.64rem 0.64rem;
          background-repeat: no-repeat;
          &.decrease {
            .bg-image('decrease_3');
          }
          &.discount {
            .bg-image('discount_3');
          }
          &.special {
            .bg-image('special_3');
          }
          &.invoice {
            .bg-image('invoice_3');
          }
          &.guarantee {
            .bg-image('guarantee_3');
          }
        }
      }
    }
  }
  .foods-wrapper {
    flex: 1;
    .title {
      height: 1.386667rem;
      background-color: #f3f5f7;
      border-left: 1px solid #d9dde1;
      font-size: 0.64rem;
      color: rgb(147, 153, 159);
      line-height: 1.386667rem;
      padding-left: 0.746667rem;
    }
    .content {
      display: flex;
      margin: 0.96rem;
      padding-bottom: 0.96rem;
      .border-bottom-1px(rgba(7, 17, 27, .1));
      .icon {
        flex: 0 0 3.04rem;
        width: 3.04rem;
        height: 3.04rem;
        margin-right: 0.533333rem;
        .icon-item {
          width: 3.04rem;
          height: 3.04rem;
        }
      }
      .desc {
        flex: 1;
        font-size: 0.32rem;
        position: relative;
        .name {
          margin-top: 0.106667rem;
          line-height: 0.746667rem;
          color: rgb(7, 17, 27);
          font-size: 0.746667rem;
        }
        .desc-item {
          padding-top: 0.426667rem;
          font-size: 0.533333rem;
          color: rgb(147, 153, 159);
        }
        .text {
          padding-top: 0.426667rem;
          font-size: 0.533333rem;
          color: rgb(147, 153, 159);
          line-height: 0.533333rem;
          .month {
            padding-right: 0.64rem;
          }
        }
        .price {
          .now {
            line-height: 1.28rem;
            font-size: 0.746667rem;
            font-weight: 700;
            color: rgb(240, 20, 20);
            padding-right: 0.4rem;
          }
          .old {
            line-height: 1.28rem;
            font-size: 0.533333rem;
            font-weight: 700;
            color: rgb(147, 153, 159);
            text-decoration: line-through;
          }
        }
        .cartcontrol-wrapper {
          position: absolute;
          right: 0;
          bottom: -0.133333rem;
        }
      }
    }
  }
}

</style>
