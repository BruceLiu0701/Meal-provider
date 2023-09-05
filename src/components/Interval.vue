<template>
        <div class="time" v-show="appear===1"> <!-- 接收到的props => appear為1則展示時間選擇區域-->
          <div class="left" @click="showLeft" :style="{borderRadius:leftRadius}" >
              <p>{{initial}}</p>
              <span class="material-symbols-outlined" :class="leftIcon">arrow_drop_down</span>
              <ol v-show="leftList" @click.stop="initialTime">
                <!-- 遍歷li, 生成48個 -->
                <li v-for="i in 48" :key="i" :class="weekday_start[i-1]==='0'?'notAllowed':''">
                    {{Math.floor((i-1)/2)<10?'0'+Math.floor((i-1)/2):Math.floor((i-1)/2)}}:{{i%2===0?'30':'00'}}
                </li> 
              </ol>
          </div>
          <p class="line">-</p>
          <div class="right" @click="showRight" :style="{borderRadius:rightRadius}">
              <p>{{end}}</p>
              <span class="material-symbols-outlined" :class="rightIcon">arrow_drop_down</span>
              <ol v-show="rightList" @click.stop="endTime">
                <!-- 遍歷li, 生成48個 -->
                <li v-for="i in 48" :key="i" :class="weekday_end[i-1]==='0'?'notAllowed':''">
                    {{Math.floor((i-1)/2)<10?'0'+Math.floor((i-1)/2):Math.floor((i-1)/2)}}:{{i%2===0?'30':'00'}}
                </li>  
              </ol>
          </div>
      </div>
</template>

<script>
    import { defineComponent } from 'vue';
    export default defineComponent({
        name:'Interval',
        props:['appear'],
        data() {
            return {
                weekday_start:'111111111111111111111111111111111111111111111111',
                weekday_end:'011111111111111111111111111111111111111111111111', // 指定資料儲存結構, 0表示不供餐時段, 1供餐
                leftList:false,              // 是否展示左方時間選擇框
                rightList:false,             // 是否展示右方時間選擇框
                initial:'00:00',             // 選擇的起始時間, 預設00:00
                end:'23:59',                 // 選擇的結束時間, 預設23:59
                leftIcon:{rotation:false},   // 左測icon選轉
                rightIcon:{rotation:false}   // 右側icon旋轉
            }
        },
        methods:{
            // 左方清單出現與隱藏
            showLeft(){
                this.leftIcon.rotation=!this.leftIcon.rotation
                this.leftList=!this.leftList
            },
            // 右方清單出現與隱藏
            showRight(){
                this.rightIcon.rotation=!this.rightIcon.rotation
                this.rightList=!this.rightList
            },
            // 選擇起始時間的函式
            initialTime(e){
                const newArr=e.target.innerHTML.split(':');
                const index=Number(newArr[0])*2+(Number(newArr[1])===30?2:1)
                let noProvider='0';
                let provider='1'
                if(this.weekday_start[index-1]!=='0'){
                    this.weekday_end='0'+noProvider.repeat(index-1).concat(provider.repeat(48-index)) //拼成新字串
                    this.initial=e.target.innerHTML
                    this.leftList=false
                    this.leftIcon.rotation=false
                }
            },
            // 選擇結束時間的函式
            endTime(e){
                const newArr=e.target.innerHTML.split(':');
                const index=Number(newArr[0])*2+(Number(newArr[1])===0?1:2)
                let noProvider='0';
                let provider='1'
                if(this.weekday_end[index-1]!=='0' && index!==1){
                    this.weekday_start=provider.repeat(index-1).concat(noProvider.repeat(48-index+1)) //拼成新字串
                    this.end=e.target.innerHTML
                    this.rightList=false
                    this.rightIcon.rotation=false
                }  
            }
        },
        computed:{
            // 左邊時段框圓角
            leftRadius(){
                return this.leftList?'0.5rem 0.5rem 0 0':'0.5rem'
            },
            // 右邊時段框圓角
            rightRadius(){
                return this.rightList?'0.5rem 0.5rem 0 0':'0.5rem'
            },

        }
    })
</script>

<style scoped>
    section .time{
        display: flex;
        justify-content: center;
        align-items: center;

    }
    /* 時間區間框的樣式*/ 
    section .time .left,
    section .time .right{
        width: 200px;
        height: 45px;
        padding: 0.6rem 0.5rem 0.6rem 1.0rem;
        border: 1px solid #aaa;
        /* border-radius: 0.5rem; */
        position: relative;
        display: flex;
        justify-content: space-between;
        align-items: center;
        cursor: pointer;
    }
    /* icon的小動畫*/ 
    section .time .left span,
    section .time .right span{
        transition: 0.2s ease-in;
    }
    section .time .line{
        margin: 0 1rem;
    }
    section .time .left p{
        width: 100%;
    }
    section .time ol{
        position: absolute;
        z-index: 2;
        background-color: #fff;
        width: 200px;
        height: 10rem;
        left: -1px;
        bottom:-10rem;
        overflow: auto;
        border: 1px solid #aaa;
        border-radius: 0 0 0.5rem 0.5rem;
    }
    section .time  ol li{
        padding: 0.6rem 0 0.6rem 1.0rem;
    }
    /* 每個時段li被摸到 */
    section .time  ol li:hover{
        background-color: #aaa;
    }
    /* 禁止選擇的樣式*/ 
    .notAllowed{
        cursor:not-allowed;
        background-color: #eee;
        color:#bbb
    }
    /* icon旋轉 */
    .rotation{
        transform: rotate(180deg);
    }
</style>