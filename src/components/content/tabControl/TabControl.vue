<template>
  <div class="tabControl">
    <div class="tabControlItem"
         v-for="(item,index) in tabControlList
">
      <span>{{item}}</span>
    </div>
  </div>

</template>

<script>
export default {
  name: "TabControl",
  props: {
    tabControlList: {
      type: Array,
      default(){
        return []
      }
    }
  }
}
</script>

<style scoped>
.tabControl {
  display: flex;
  text-align: center;
  font-size: 15px;
  height: 40px;
  line-height: 40px;
  background-color: #fff;
}
.tabControlItem {
  flex: 1;
}
.tab-control-item span {
  padding: 5px;
}

.active {
  color: var(--color-high-text);
}

.active span {
  border-bottom: 3px solid var(--color-tint);
}
</style>
