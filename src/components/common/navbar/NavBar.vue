<template>
    <div class="navbar">
        <div class="left">
            <slot name="left"></slot>
        </div>
        <div class="center">
            <slot name="center"></slot>
        </div>
         <div class="right">
            <slot name="right"></slot>
        </div>
    </div>
</template>
<script>
export default {
    name: 'NarBar'
}
</script>
<style scoped>
    .navbar {
        display: flex;
        line-height: 44px;
        height: 44px;
        box-shadow: 0px -1px 1px rgba(100,100,100,.2);
        text-align: center;
        
    }
    .left,.right {
        width: 60px;
    }
    .center {
        flex: 1;
    }
</style>