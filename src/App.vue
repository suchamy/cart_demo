<template>
  <div class="app-container">
    <!-- <h1>App 根组件</h1> -->
    <Header v-text="'购物车案例'"></Header>
    <Goods v-for="item in goods" 
    :key="item.id" 
    :title="item.goods_name" 
    :pic="item.goods_img"
    :state="item.goods_state"
    :price="item.goods_price"
    ></Goods>
    <Footer :init="goods"></Footer>
  </div>
</template>

<script>
import Header from './components/Header/Header.vue'
import axios from 'axios'
import Goods from './components/Goods/Goods.vue'
import Footer from './components/Footer/Footer.vue'

export default { 
  data(){
    return {
      goods:[]
    }
  },
  components: {
    Header,
    Goods,
    Footer
},
  methods:{
    async initCartList(){
      const { data:result } = await axios.get('https://www.escook.cn/api/cart')
      console.log(result)
      this.goods = result.list
    },
  },
  created(){
    this.initCartList()
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
