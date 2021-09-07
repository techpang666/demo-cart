<template>
  <div class="app-container">
    <!-- 标题部分 -->
    <!-- 绑定标题变量 -->
    <Header :title="title"></Header>
    <!-- 商品列表部分 -->
    <!-- <Goods v-for="item in list" :key="item.id" :name="item.goods_name"></Goods> -->
  </div>
</template>

<script>
import Header from '@/components/Header/Header'
import Goods from '@/components/Goods/Goods'
import axios from 'axios'
export default {
  components: {
    Header,
    Goods
  },
  data() {
    return {
      // 标题
      title: '购物车案例',
      // 商品列表
      list: []
    }
  },
  methods: {
    async getGoodsList() {
      const { data } = await axios.get('https://www.escook.cn/api/cart')
      // 如果请求成功 把数据存到data中
      if (data.status === 200) {
        this.list = data.list
      }
    }
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
