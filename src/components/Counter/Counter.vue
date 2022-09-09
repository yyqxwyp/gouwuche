<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{num}}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
  import bus from '../EventBus'
export default {
  methods:{
    add(){
this.$emit('num-change',this.num+1)
    }
  },
  props:{
    num:{
      type:Number,
      default:1
    },
    // 接收商品的id 将来使用eventbus把数量传递到App.bus的时候 需要APP
    id:{
      type:Number,
      required:true
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
