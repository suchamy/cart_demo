<template>
  <div class="app-container">
    <!-- <h1>App 根组件</h1> -->
    <Header v-text="'购物车案例'"></Header>
    <Goods 
      v-for="item in goods" 
      :key="item.id" 
      :id="item.id"
      :title="item.goods_name" 
      :pic="item.goods_img"
      :state="item.goods_state"
      :price="item.goods_price"
      :count="item.goods_count"
      @state="getState"
    ></Goods>

    <Footer 
      :fullState="fullState" 
      :totalPrice="goodsPrice"
      :totalNum="goodsNum"
      @full-state-change="getFullState" 
    ></Footer>
  </div>
</template>

<script>
import axios from 'axios'
import Header from './components/Header/Header.vue'
import Goods from './components/Goods/Goods.vue'
import Footer from './components/Footer/Footer.vue'
import bus from '@/components/eventBus.js'

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
    getState(obj){
      this.goods.some(item => {
        if(item.id === obj.id){
          item.goods_state = obj.value
          return true
        }
      })
    },
    getFullState(val){
      this.goods.forEach(item => item.goods_state = val)
    },
  },
  computed:{
    fullState(){
      return this.goods.every(item => item.goods_state)
    },
    goodsPrice(){
      return this.goods.filter(item => item.goods_state)
      .reduce((previous,current) => previous + current.goods_count * current.goods_price,0)
    },
    goodsNum(){
      let newArr = this.goods.filter(item => item.goods_state === true)
      return newArr.reduce((previous,current) => previous + current.goods_count,0)
    }
  },
  created(){
    this.initCartList()
    bus.$on(['addCount','subCount'], obj => {
      this.goods.some(item => {
        if(item.id === obj.id){
          item.goods_count = obj.value
          return true
        }
      })
    })
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
