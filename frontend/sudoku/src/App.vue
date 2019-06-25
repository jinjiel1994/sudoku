<template>
  <div id="app">
<!--    <div class="number-container">-->
<!--      <div class="select-number" v-for="(number,i) in numbers">{{number}}</div>-->
<!--    </div>-->
    <div class="sudoku-board">
      <div v-for="(row,i) in board" class="board-row">
        <input v-for="(grid,j) in row" class="board-grid" :value="grid" @input="inputChange($event,i,j)"/>
      </div>
    </div>
    <button class="button" @click="output">解数独</button>
    <button style="left: 55%;" class="button" @click="reset">重置</button>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';


export default {
  name: 'app',
  components: {
  },
  data() {
    return {
      board: [[0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0]],
      numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9],
    };
  },
  methods: {
    output() {
      console.log(this.board);

      // POST /someUrl
      this.$http.post(window.RestfulSudoku, {
        data: this.board,
        header: { 'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8', 'Access-Control-Allow-Origin': '*' },
      }).then((response) => {
        this.board = response.data.data;
      }, (response) => {
        alert(response);
      });
    },
    reset() {
      // eslint-disable-next-line max-len
      this.board = [[0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 0]];
    },
    inputChange($event, i, j) {
      const value = Number($event.target.value);
      if (isNaN(value)) {
        this.$set(this.board[i], j, '');
      } else if ((value < 10) && (value > 0)) {
        this.$set(this.board[i], j, value);
      } else {
        // eslint-disable-next-line max-len
        this.$set(this.board[i], j, Number(String(value).substring(String(value).length - 1, String(value).length)));
      }
    },
  },
};
</script>

<style>
#app {
}

.sudoku-board{
  width: 27rem;
  height: 27rem;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  border: black 1px solid;
}

.board-row {
  width: 100%;
  height: 2.99rem;
  display: flex;
}

.board-grid {
  width: 2.99rem;
  height: 100%;
  text-align: center;
  font-size: 1.5rem;
  line-height: 2.8rem;
}

.selected {
  border: #42b983 1px solid;
}

.number-container {
  display: flex;
  position: absolute;
  left: 50%;
  top: 8%;
  -webkit-transform: translate(-50%);
  -moz-transform: translate(-50%);
  -ms-transform: translate(-50%);
  -o-transform: translate(-50%);
  transform: translate(-50%);
}

.select-number {
  width: 3rem;
  height: 3rem;
  border: aqua 1px solid;
  border-radius: 50%;
  text-align: center;
  line-height: 3rem;
}

.button {
  position: absolute;
  left: 50%;
  top: 89%;
  transform: translate(-50%,-50%);
}
</style>
