<template>
<body>
  <div class="wrapper">
    <div  v-if="currentTime === 'day'">
      <transition name="slide-fade">
        <table style="width:100%" v-if="!justUpdated">
          <tr>
            <th>合計</th>
            <th>97.5%</th>
            <th>2.5%</th>
          </tr>

          <tr>
            <th></th>
            <th>その他</th>
            <th>ラッキー</th>
          </tr>
          <tr class='btn' style="backgroundColor:CornflowerBlue">
            <th></th>
            <th @click="dayCountingList.first = dayCountingList.first + 1">+1</th>
            <th @click="dayCountingList.fourth = dayCountingList.fourth + 1; lastTime= dayTotal">+1</th>
          </tr>
          <tr class='btn' style="backgroundColor:LightCoral">
            <th></th>
            <th @click="dayCountingList.first = dayCountingList.first + 5">+5</th>
            <th @click="dayCountingList.second = dayCountingList.second + 10">+10</th>
          </tr>
          <tr >
            <th>{{dayTotal}}</th>
            <th>{{dayTotal - dayCountingList.fourth}}</th>
            <th>{{dayCountingList.fourth}}</th>
          </tr>
          <tr class='empty'>
            <th>&nbsp;</th>
            <th></th>
            <th></th>
          </tr>
          <tr >
            <th>100%</th>
            <th>{{100 -chanceList.fourth}}%</th>
            <th>{{chanceList.fourth}}%</th>
          </tr>

        </table>
        
        <div class="container" v-else>
          <div>
            <h2  class='message'>Just Updated!</h2>
          </div>
        </div>
        
      </transition>


      <h5 v-if="!justUpdated">{{dayTotal - lastTime}} pokemon since Lasgt appearence of Lucky </h5>
    </div>
  <div v-else>
    <h1>yo</h1>
    <span>time: {{currentTime}}</span>
  </div>
  </div>
  
</body>
</template>

<script>
// import func from 'vue-editor-bridge'

export default {
  name: 'App',
  data(){
    return{
      dayCountingList: {first:0,second:0,third:0,fourth:0},
      nightCountingList: {first:0,second:0,third:0,fourth:0},
      currentTime: 'day',
      name:'nozo',
      developing: true,
      countingSince: 0,
      lastTime: 0,
      justUpdated: false,
      

    }
  },
  mounted(){
    if(this.developing){
      this.currentTime = 'day'
    }

  },
  created(){
  // localStorage.firstTime = JSON.stringify(this.candidate); 
  if(localStorage.dayCountingList){
    this.dayCountingList = JSON.parse(localStorage.dayCountingList);
  }else{
    console.log('welcome new user')
  }

  if(localStorage.lastTime){
    this.lastTime = JSON.parse(localStorage.lastTime);
  }
  
    // console.log(this.candidate[1])
  },
  methods:{
    getChance(num){
      if(num){
        let theNum = (num / this.dayTotal) * 100
        theNum = Math.floor(theNum)
        return theNum
      }else{
        return ''
      }

      
    },
  },
  computed:{
    dayTotal: function(){
      let count = 0
      count = this.dayCountingList.first 
      count = count + this.dayCountingList.second
      count = count + this.dayCountingList.third
      count = count + this.dayCountingList.fourth
      return count
    },
    chanceList: function(){
      let list= {first: this.getChance(this.dayCountingList.first), second:this.getChance(this.dayCountingList.second), third:this.getChance(this.dayCountingList.third), fourth:this.getChance(this.dayCountingList.fourth)}
      return list
    },
  },
  watch:{

    dayCountingList: {
        deep:true,
        handler() {
          localStorage.dayCountingList = JSON.stringify(this.dayCountingList);
          console.log('changed')
          this.justUpdated = true
          setTimeout(() =>{
            console.log('been 1 sec')
            this.justUpdated = false
          },1000)
        }
      },
      lastTime: function(){
        localStorage.lastTime = JSON.stringify(this.lastTime);
      }
  },
    

}
</script>

<style>

.message{
  position: absolute;
  top: 35%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 400%;
  color: darkgreen;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

table, th, td {
  border:1px solid black;
}

.btn{
  height: 225px;
}
.empty{
  height: 50px;
}

.slide-fade-enter-active {
  transition: all 0.5s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
</style>
