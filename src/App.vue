<template>
  <div class="board">
    <div
      v-for="(tile, index) in tiles"
      :key="index"
      class="tile"
      :class="{ empty: tile === 0 }"
      @click="moveTile(index)">
      {{ tile || '' }}
    </div>
  </div>
</template>

<script>

export default {
  props: {
  },
  data() {
    return {
      tiles: []
    };
  },
  created(){
    this.tiles = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,0].sort(() => 0.5 - Math.random());
  }, 
  methods: {
    moveTile(index) {
      if (this.canMoveTile(index)) {
        this.swapTiles(index);
        if (this.isSolved()) {
          alert('Solved!');
        }
      }
    },
    canMoveTile(index) {
      const emptyIndex = this.tiles.indexOf(0);
      const rowDiff = Math.abs(Math.floor(emptyIndex / 4) - Math.floor(index / 4));
      const colDiff = Math.abs((emptyIndex % 4) - (index % 4));
      return (rowDiff === 1 && colDiff === 0) || (rowDiff === 0 && colDiff === 1);
    },
    swapTiles(index) {    
      const emptyIndex = this.tiles.indexOf(0);
      let prev_index = this.tiles[index];
      this.tiles[index] = this.tiles[emptyIndex];
      this.tiles[emptyIndex] = prev_index;
      this.$forceUpdate();
    },
    isSolved() {
      return this.tiles.filter(t => t !== 0).every((tile, index) => tile === index + 1);
    }
  },
};
</script>

<style scoped>
.board {
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  width: 480px;
  height: 480px;
}

.tile {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2.5rem;
  background-color: #f0f0f0;
  cursor: pointer;
  user-select: none;
}

.empty {
  background-color: #ddd;
}
</style>
