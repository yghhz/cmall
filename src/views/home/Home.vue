<template>
<div id="home">
  <nav-bar class="home-nav">
    <div slot="center">购物街</div>
  </nav-bar>
  <home-swiper :banners="banners"></home-swiper>
  <home-recommends :recommends="recommends"></home-recommends>
  <home-favorite/>
</div>
</template>

<script>
import NavBar from 'components/common/navbar/NavBar'

import HomeSwiper from './childComps/HomeSwiper'
import HomeRecommends from './childComps/HomeRecommends'
import HomeFavorite from './childComps/HomeFavorite'

import {getMultiData}  from 'network/home'

  export default {
    name: "Home",
    data () {
      return {
        recommends:[],
        banners:[]
      }
    },
    components: {
      NavBar,
      HomeSwiper,
      HomeRecommends,
      HomeFavorite
    },
    created () {
      getMultiData().then(res =>{
         this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
      })
    }
  }
</script>

<style scoped>
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
</style>
