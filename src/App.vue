<template>
  <div class="app-container">
    <!-- 标题部分 -->
    <!-- 绑定标题变量 -->
    <Header :title="title"></Header>
    <!-- 商品列表部分 -->
    <Goods
    v-for="item in list"
    :key="item.id"
    :id="item.id"
    :name="item.goods_name"
    :pic="item.goods_img"
    :price="item.goods_price"
    :count="item.goods_count"
    :state="item.goods_state"
    @state-change="changeState"
    >
    </Goods>
    <!-- 结算按钮 -->
    <Footer :isFull="fullState" :total="sum" @full-change="getFullChange"></Footer>
  </div>
</template>

<script>
import Header from '@/components/Header/Header'
import Goods from '@/components/Goods/Goods'
import Footer from '@/components/Footer/Footer'
import axios from 'axios'
import bus from '@/components/eventBus.js'
export default {
  components: {
    Header,
    Goods,
    Footer
  },
  created() {
    // 获取商品列表数据
    this.getGoodsList()
    
    // 通过eventBus接收数据
    bus.$on('share', val => {
      this.list.some(item => {
        if (item.id === val.id) {
          item.goods_count = val.value
          return true
        }
      })
    })
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
    },
    // 勾选商品改变全选按钮
    changeState(e) {
      // some有一个元素达到条件进行函数处理
      this.list.some(item => {
        if(item.id === e.id) {
          // 修改勾选状态
          item.goods_state = e.value
          // 如果id相同 终止循环
          return true
        }
      })
    },
    // 完成全选功能
    getFullChange(e) {
      this.list.forEach(item => {
        return item.goods_state = e
      });
    }
  },
  computed: {
    fullState() {
      // 加括号需要return出去
      return this.list.every(item => {return item.goods_state})
    },
    sum() {
      // 先过滤已勾选的商品 再叠加总价
      return this.list.filter(item => item.goods_state).reduce((total, item) => total += item.goods_price * item.goods_count, 0)
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
