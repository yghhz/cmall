<template>
    <div>
    <nav-bar>
        <div slot="left" class="back" @click="backClick">
            <img src="~assets/img/common/back.svg" alt="">
        </div>
        <div slot="center" class="center">
            <div class="center-item" v-for="(item,index) in titles"  @click="navBarClick(index)" :key='item' :class="{active: index === currentIndex}">
                {{item}}
            </div>    
        </div>
    </nav-bar>
    </div>
</template>
<script>
import NavBar from 'components/common/navbar/NavBar'
export default {
    name: 'DetailNavBar',
    data () {
        return {
             titles: ['商品', '参数', '评论', '推荐'],
             currentIndex:0
        }
    },
    components: {
        NavBar
    },
    methods: {
        backClick(){
            this.$router.back()
        },
        navBarClick(index){
            this.currentIndex = index;
        }
    }
}
</script>
<style scoped>
    .center{
        display: flex;
        font-size: 13px;
    }
    .center-item{
        flex: 1;
    }
    .active {
        color: var(--color-high-text);
    }
    .back img{
        margin-top: 12px;
    }
</style>