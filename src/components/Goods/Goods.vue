<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <input type="checkbox" ref="ckBox" class="custom-control-input" :id="'cb'+id" :checked="state" @click="changeState"/>
        <label class="custom-control-label" :for="'cb'+id">
          <!-- 商品的缩略图 -->
          <img :src="pic" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ title }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{ price }}</span>
        <!-- 商品的数量 -->
        <Counter :counter="count" :id="id"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
import Counter from '../Counter/Counter.vue'
export default {
    props: {
      id:{
        type:Number,
        required:true
      },
      title:{
        default:'标题',
        type: String
      },
      pic:{
        default:'',
        type: String
      },
      price:{
        default:0,
        type:Number
      },
      state:{
        default:false,
        type:Boolean
      },
      count:{
        default: 0,
        type: Number
      }
    },
    components: { Counter },
    methods:{
      getNewCount(val){
        this.goods.goods_count = val
      },
      changeState(){
        this.$emit('state',{
          id:this.id,
          value:this.$refs.ckBox.checked
      })
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
