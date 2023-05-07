<template>
  <div id="app">
    <h1>Pairs Game</h1>
    <label for="board-size">Board size: </label>
    <select id="board-size" v-model="boardSize">
      <option value=2>2x2</option>
      <option value=4>4x4</option>
    </select>
    <GameBoard :board-size="boardSize" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import GameBoard from "@/components/GameBoard.vue";

export default defineComponent({
  name: 'App',
  components: {
    GameBoard,
  },
  data() {
    return {
      boardSize: 4,
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}

select#board-size {
  background-color: #ffffff;
  border: 2px solid #2c3e50;
  border-radius: 6px;
  color: #2c3e50;
  font-size: 18px;
  padding: 4px 8px;
  margin-left: 10px;
  margin-bottom: 20px;
}
</style>
