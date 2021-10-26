<template>
<body>
  <div class="wrapper">
    <div  v-if="currentTime === 'day'">
      <table style="width:100%">
        <tr>
          <th>合計</th>
          <th>50%</th>
          <th>25%</th>
          <th>22.5%</th>
          <th>2.5%</th>
        </tr>

        <tr>
          <th></th>
          <th>ピジョン</th>
          <th>ハネッコ</th>
          <th>ポポッコ</th>
          <th>ラッキー</th>
        </tr>
        <tr class='btn' style="backgroundColor:CornflowerBlue">
          <th></th>
          <th @click="dayCountingList.first = dayCountingList.first + 1">+1</th>
          <th @click="dayCountingList.second = dayCountingList.second + 1">+1</th>
          <th @click="dayCountingList.third = dayCountingList.third + 1">+1</th>
          <th @click="dayCountingList.fourth = dayCountingList.fourth + 1">+1</th>
        </tr>
        <tr class='btn' style="backgroundColor:LightCoral">
          <th></th>
          <th @click="dayCountingList.first = dayCountingList.first + 5">+5</th>
          <th @click="dayCountingList.second = dayCountingList.second + 5">+5</th>
          <th @click="dayCountingList.third = dayCountingList.third + 5">+5</th>
          <th @click="dayCountingList.fourth = dayCountingList.fourth + 5">+5</th>
        </tr>
        <tr >
          <th>{{dayTotal}}</th>
          <th>{{dayCountingList.first}}</th>
          <th>{{dayCountingList.second}}</th>
          <th>{{dayCountingList.third}}</th>
          <th>{{dayCountingList.fourth}}</th>
        </tr>
        <tr class='empty'>
          <th>&nbsp;</th>
          <th></th>
          <th></th>
          <th></th>
          <th></th>
        </tr>
        <tr >
          <th>100%</th>
          <th>{{chanceList.first}}%</th>
          <th>{{chanceList.second}}%</th>
          <th>{{chanceList.third}}%</th>
          <th>{{chanceList.fourth}}%</th>
        </tr>

      </table>
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
      developing: true
      

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
      }
    },
  }
}
</script>

<style>
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
  height: 250px;
}
.empty{
  height: 50px;
}
</style>
