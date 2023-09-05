<template>
    <div class="date">
        <h3>星期{{date}}</h3>
        <div class="selector" :style="{backgroundColor:power}" @click="changeStyle">
            <p :class="move"></p>
        </div>
        <p>本日{{provide}}</p>
    </div>
</template>

<script>
    import { defineComponent } from 'vue';
    export default defineComponent({
        name:'Meal',
        props:['number','changeAppear'],
        data() {
            return {
                power:'',
                move:''
            }
        },
        methods:{
            // 點擊供餐開關後的樣式新增或移除
            changeStyle(){
                if(this.power&&this.move){
                    this.power='';
                    this.move='';
                    this.changeAppear(0,this.number);
                }else{
                    this.power='rgb(71, 165, 101)';
                    this.move='slide';
                    this.changeAppear(1,this.number);
                }
            }
        },
        computed:{
            // 利用傳入的props, 動態生成星期一 ~ 日
            date(){
                const {number}=this;
                return number===1?'一':number===2?'二':number===3?'三':number===4?'四':number===5?'五':number===6?'六':'日'
            },
            // 切換顯示供餐還是不供餐
            provide(){
                return this.power&&this.move?'供餐':'不供餐'
            }
        }
    })
</script>

<style scoped>
    section .date{
        flex:1;
        min-width: 220px;
        height: 45px;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-right: 5rem;
    }
    section .date h3{
        margin-right: 2rem;
        font-size: 16px;
        font-weight: 700;
    }
    section .date p{
        font-size: 16px;
        font-weight: 700;
        flex:1;
    }
    section .date .selector{
        width: 3rem;
        height: 1.5rem;
        background-color: #ccc;
        border-radius: 2rem;
        position: relative;
        cursor: pointer;
        margin-right: 0.6rem;
    }
    section .date .selector p{
        position: absolute;
        width: 1.3rem;
        height: 1.3rem;
        background-color: #fff;
        border-radius: 50%;
        left: 0.1rem;
        top: 0.1rem;
        transition: 0.2s linear;
    }
    .slide{
        transform: translateX(1.5rem);
    }
</style>