<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <input type="checkbox" class="custom-control-input" :id="'cb' + id" :checked="state" @change="getState" />
        <!-- 动态处理对应的商品状态 -->
        <label class="custom-control-label" :for="'cb' + id">
          <!-- 商品的缩略图 -->
          <img :src="pic" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ name }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">¥{{ price.toFixed(2) }}</span>
        <!-- 商品的数量 -->
        <Counter :num="count" :id="id"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
import Counter from '@/components/Counter/Counter'
export default {
  components: {
    Counter
  },
  props: {
    // 父组件需要根据id去修改勾选状态
    id: {
      // 必传属性 不需要default
      required: true,
      type: Number,
    },
    name: {
      type: String,
      default: ''
    },
    pic: {
      type: String,
      default: ''
    },
    price: {
      type: Number,
      default: 0
    },
    state: {
      type: Boolean,
      default: true
    },
    count: {
      type: Number,
      default: 1
    }
  },
  methods: {
    getState(e) {
      const newState = e.target.checked
      // 通过this拿到props的id值
      // 给父组件传值
      this.$emit('state-change', {id: this.id, value: newState})
    }
  }
}
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
