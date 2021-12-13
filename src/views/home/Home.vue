<template>
<div id="home">
  <nav-bar class="home-nav">
    <div slot="center">购物街</div>
  </nav-bar>
  <tab-control class="tab-control" :tabControlList="['流行', '新款', '精选']" @tabClick="tabClick" ref="tabControl1" v-show="isTabFixed"></tab-control>
  <scroll class="content" :probeType="probeType" :pullUpLoad="pullUpLoad" @scroll="contentScroll" @pullingUp="loadMore" ref="scroll">
    <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
    <home-recommends :recommends="recommends"></home-recommends>
    <home-favorite/>
    <tab-control :tabControlList="['流行', '新款', '精选']" @tabClick="tabClick" ref="tabControl2"></tab-control>
    <good-list :goods="showGoods"/>
  </scroll>
  <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>
</div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'
import Scroll from 'components/common/scroll/Scroll'

import HomeSwiper from './childComps/HomeSwiper'
import HomeRecommends from './childComps/HomeRecommends'
import HomeFavorite from './childComps/HomeFavorite'

import GoodList from 'components/content/goods/GoodsList'
import TabControl from "components/content/tabControl/TabControl";
import BackTop from 'components/content/backTop/BackTop'

import {debunce} from "common/utils";
import {getMultiData,getHomeGoods}  from 'network/home'
  export default {
    name: "Home",
    data () {
      return {
        recommends:[],
        banners:[],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []},
        },
        currentType:'pop',
        probeType:3,
        pullUpLoad:true,
        isShowBackTop:false,
        offsetTop:0,
        isTabFixed:false,
        saveY: 0
      }
    },
    components: {
      NavBar,
      Scroll,
      HomeSwiper,
      HomeRecommends,
      HomeFavorite,
      GoodList,
      TabControl,
      BackTop
    },
    computed: {
      showGoods(){
        return this.goods[this.currentType].list;
      }
    },
    created () {
      // 1.请求多个数据
      this.getHomeMultidata()

      // 2.请求商品数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')

    },
    deactivated () {
      this.saveY = this.$refs.scroll.getScrollY();
    },
    activated () {
     this.$refs.scroll.scrollTo(0, this.saveY, 0)
     this.$refs.scroll.refresh()
    },
    mounted () {
      const refresh = debunce(this.$refs.scroll.refresh, 50)
      this.$bus.$on('itemImageLoad',() =>{
        refresh()
      })
    },
    methods: {
      getHomeMultidata(){
        getMultiData().then(res =>{
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1

          this.$refs.scroll.finishPullUp()
        })
      },
      tabClick(index){
         switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
      },
      backClick(){
        this.$refs.scroll.scrollTo(0,0)
      },
      contentScroll(position){
          this.isShowBackTop = (-position.y) > 1000
          
          // 2.决定tabControl是否吸顶(position: fixed)
        this.isTabFixed = (-position.y) > this.offsetTop
      },
      swiperImageLoad(){
          this.offsetTop = this.$refs.tabControl2.$el.offsetTop;
      },
      loadMore(){
        this.getHomeGoods(this.currentType)
      }
    }
  }
</script>

<style scoped>
.home-nav {
  background-color: var(--color-tint);
  color: #fff;

  /* position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9; */
}
/* .tabControl {
  position: sticky;
  top: 44px;
  z-index: 9;
} */
  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
    .tab-control {
    position: relative;
    z-index: 9;
  }
</style>
