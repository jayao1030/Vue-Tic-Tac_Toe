<template>
  <div class="app">
    <div class="board">
      <div class="grid"
      v-for= "(grid,idx) in grids"
      :key="(grid,idx)"
      @click="setGrid(idx)"
      >{{ getSymbol(grid)}}

      </div>
    </div>
    <div class="info">
      <span>Player: {{ getSymbol(player)}}</span>
      <span>Winner: {{ getSymbol(winner)}}</span>
      <button @click="reset">Reset</button>
    </div>
  </div>
</template>

<script>

// 獲勝的所有條件
const lines = [
  [0,1,2],[3,4,5],[6,7,8],
  [0,3,6],[1,4,7],[2,5,8],
  [0,4,8],[2,4,6]
]

export default {
  data(){
    return{
      //九個格子，預設為0
      grids:[
        0,0,0,
        0,0,0,
        0,0,0,
      ],
      player:1,
      // winner:0
    };
  },
  computed:{
    winner(){
      //reduce() 把陣列裡的每個元素減少成單一結果
      return lines.reduce((winner,[a,b,c]) => {
        if(winner !==0) return winner;
        const sum = this.grids[a] + this.grids[b] + this.grids[c];
        if(sum === 3) return 1;
        if(sum === -3) return -1;
        return 0;
      },0)
    }
  },
  methods:{
    setGrid(idx){
      //不等於0就return ， 防止連續切換同一格
      if(this.grids[idx] !== 0) return;
      //
      if(this.winner !== 0) return; 
      // 以陣列索引值的方式修改資料內容時，Vue 無法監測到
      // 利用vm.$set(array, index, value)方法)
      this.$set(this.grids,idx,this.player);
      //切換player 1 跟 -1
      this.player = -this.player;
     
    },
    //把1與-1轉換成O與X
    getSymbol(num){
      return num === 0 ? '': num === 1 ? 'O':'X';
    },

    //重置
    reset(){
      this.grids = [0,0,0,0,0,0,0,0,0];
      this.player = 1;
    }
  }
}
</script>

<style>
  .app{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    height: 100vh;
  }
  .board{
    display: flex;
    flex-flow: row wrap;
    align-content: flex-start;
    width: 400px;
    height: 400px;

  }
  .grid{
    width: 33%;
    height: 33%;
    border: 1px solid #487;
    box-sizing: border-box;
    text-align: center;
    line-height: 130px;
    font-size: 80px;
    cursor: pointer;
  }
  .info{
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 30px;
    padding: 20px;
  }
  .info button{
    margin: 10px 0;
    border:2px solid #487;
    background-color: #fff;
    cursor: pointer;
  }
</style>