<template>
  <div class="header-container">{{title}}</div>
</template>

<script>
export default {
  props: {
    // 自定义标题属性 通过父组件传值
    title: {
      type: String,
      default: ''
    },
  }
}
</script>

<style lang="less" scoped>
.header-container {
  font-size: 12px;
  height: 45px;
  width: 100%;
  background-color: #1d7bff;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  position: fixed;
  top: 0;
  z-index: 999;
}
</style>
