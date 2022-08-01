<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ counter }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from '@/components/eventBus.js'

export default {
  props:{
    id:{
    type: Number,
    required: true
  },
    counter:{
      default: 0,
      type: Number
    }
  },
  methods:{
    add(){
      bus.$emit('addCount',{ id: this.id, value: this.counter + 1 })
    },
    sub(){
      if(this.counter >= 1) {
        bus.$emit('subCount',{ id:this.id, value: this.counter - 1 })
      }
    }
  }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
