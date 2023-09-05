
<template>
  <section v-for="i in 7" :key="i">
      <Meal :number="i" :changeAppear="changeAppear"/>
      <Interval :number="i" :appear="appear[i-1]"/>
  </section>  
</template>

<script>
    import {defineComponent} from 'vue'
    import Interval from './components/Interval.vue'
    import Meal from './components/Meal.vue'
    export default defineComponent({
      name:'App',
      components:{
        Interval,
        Meal
      },
      data() {
        return {
          appear:[0,0,0,0,0,0,0]   // 時間區間是否顯示的數據, 每個0分別代表不同section的時間區間
        }
      },
      methods:{
        // 接收到參數後更改appear, 以此決定星期幾的時間選擇區塊要展示
        changeAppear(state,num){
          this.appear[num-1]=state
        }
      }
    })
</script>

<style>
  section{
      width: 740px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      position: relative;
      padding: 2.5rem 0;
    }
    section::after{
      content: "";
      position: absolute;
      width: 100%;
      border-bottom: 1px solid #555;
      left: 0;
      bottom: 0;
    }
</style>
