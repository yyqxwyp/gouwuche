<template>
  <div class="app-container">
    <!-- <h1>App 根组件</h1> -->
    <!-- 头部 -->
    <Header></Header>
    <!-- 商品区域 -->
<Goods 
v-for="item in list" 
:key="item.id"  
:title="item.goods_name" 
:pic="item.goods_img"
 :price="item.goods_price" 
 :state="item.goods_state"
 :id="item.id"
 :count="item.goods_count"

 @state-change="getNewState"
 >
 <Counter :num="item.goods_count" @num-change="getNewNum(item,$event)">

 </Counter>
</Goods>

 <!-- 底部 -->
 <Footer :isfull="fullState" :amount="amt" :all="total" @full-change="getFullState"></Footer>
  </div>
</template>

<script>
  // 导入需要的组件
  import Header from './components/Header/Header.vue'
  import axios from 'axios'
  import Goods from './components/Goods/Goods.vue'
  import Footer from './components/Footer/Footer.vue'
  import bus from './components/EventBus'
  import Counter from './components/Counter/Counter.vue'

export default {

  data(){
return{
  // 用来存储购物车的列表数据 默认为空数组
  list:[]
}
  },
 
  methods:{
    // /获取counter组件发过来的最新的数量之
    getNewNum(item,e){
// console.log(item,e);
item.goods_count=e

    },
    
// 封装请求数据列表的方法
async initCartList(){
  // 调用axios的get方法 请求流标数据
const {data:res}=await  axios.get('https://www.escook.cn/api/cart')
// 只要请求回来的数据 在页面中要用到的 必须转存到data中
// console.log(res);
if(res.status===200){
  this.list=res.list
}
},

// 接收子组件传递过来的数据
    // e的格式为（id,value)
    getNewState(e){
// console.log(e);
this.list.some(item=>{
  if(item.id===e.id){
    item.goods_state=e.value
  // 终止后续的循环
  return true
  }
 
})
    },

    getFullState(val){
      this.list.forEach(item=>item.goods_state=val)
      // console.log('ok');
      // console.log(val);
    }

  },
  components:{
    Header,
    Goods,
    Footer,
    Counter
  },
  created(){
// 调用请求数据的方法
this.initCartList()
bus.$on('share',val=>{
      // console.log(val);
      this.list.some(item=>{
        if(item.id===val.id){
          item.goods_count=val.value
          return true
        }
      })
    })
  },
   // 计算属性
   computed:{
    // 动态计算出全选的状态是true还是false
fullState(){
  return this.list.every(item=>item.goods_state)

},
 // 已勾选商品的总价格
 amt(){
    return this.list
    // 1 先过滤
.filter(item=>item.goods_state)
.reduce((total,item)=>{
  return total+=item.goods_price*item.goods_count
},0)
  },
   // 已勾选商品的总数量
   total(){
   return   this.list.filter(item=>item.goods_state).reduce((t,item)=>{
  return t+item.goods_count
      },0)
    },
  },
 
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
