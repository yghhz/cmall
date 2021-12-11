<template>
<div id="home">
  <nav-bar class="home-nav">
    <div slot="center">购物街</div>
  </nav-bar>
  <home-swiper :banners="banners"></home-swiper>
  <home-recommends :recommends="recommends"></home-recommends>
  <home-favorite/>
  <tab-control class="tabControl" :tabControlList="['流行', '新款', '精选']"></tab-control>
  <ul>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
    <li>ces</li>
  </ul>
  <!--<good-list :goods="showGoods"/>-->
</div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'

import HomeSwiper from './childComps/HomeSwiper'
import HomeRecommends from './childComps/HomeRecommends'
import HomeFavorite from './childComps/HomeFavorite'

import GoodList from 'components/content/goods/GoodsList'
import TabControl from "components/content/tabControl/TabControl";

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
        currentType:0
      }
    },
    components: {
      NavBar,
      HomeSwiper,
      HomeRecommends,
      HomeFavorite,
      GoodList,
      TabControl
    },
    computed: {
      showGoods(){
        return this.goods['pop'].list;
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
        })
      }
    }
  }
</script>

<style scoped>
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
.tabControl {
  position: sticky;
  top: 44px;
  z-index: 9;
}
</style>
