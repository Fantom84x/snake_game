<template>
  <div
    class="d-flex flex-column justify-center align-center"
    style="width: 100%; height: 100%"
  >

    <div
      id="gridBorder"
      style="position: relative"
    >
      <div
        v-for="(row, i) in gridArr"
        :key="i + 'A'"
        class="d-flex"
      >
        <div
          v-for="(col, k) in row"
          :key="k + 'A'"
        >

          <div
            :id="col.blockCoords"
            class="b3 d-flex justify-center align-center"
            style="width: 50px; height: 50px"
          >
            {{ col.blockCoords }}
          </div>

        </div>
      </div>


      <template
        v-for="(snakeBlock, i) in snakeArr"
        :key="i + 'A'"
      >
        <div
          style="position: absolute; height: 48px; width: 48px; background-color: rgba(0, 128, 0, 75%)"
          :style="`top: ${snakeBlock.top}px; left: ${snakeBlock.left}px`"
        ></div>
      </template>

    </div>

  </div>

</template>

<script setup>
import {onMounted, ref} from "vue";

const gridArr = ref([]);
const snakeArr = ref([]);
const gridBorder = ref(null);

function getKeyPress(event) {
  switch (event.keyCode) {
    case 37: // Left
      moveSnake(0, -50);
      break;
    case 38: // Up
      moveSnake(-50, 0);
      break;
    case 39: // Right
      moveSnake(0, 50);
      break;
    case 40: // Down
      moveSnake(50, 0);
      break;
  }
}

function moveSnake(topAdjustmentVal, leftAdjustmentVal) {

  console.log(snakeArr.value);

  // snakeArr.value[0].top += topAdjustmentVal;
  // snakeArr.value[0].left += leftAdjustmentVal;
  // snakeArr.value[1].top += topAdjustmentVal;
  // snakeArr.value[1].left += leftAdjustmentVal;

  const newSnakeArr = [];

  for (let i = 0; i < snakeArr.value.length; i++) {
    if (i === 0) {
      newSnakeArr.push({
        top: snakeArr.value[i].top + topAdjustmentVal,
        left: snakeArr.value[i].left + leftAdjustmentVal,
      });
      continue;
    }

    newSnakeArr.push({
      top: snakeArr.value[i - 1].top,
      left: snakeArr.value[i - 1].left,
    });

  }

  console.log(newSnakeArr);

  snakeArr.value = newSnakeArr;

}

function chooseSnakeStartingHeadPositionOnGrid() {
  const randomPosRow = Math.floor(Math.random() * gridArr.value.length).toString();
  const randomPosCol = Math.floor(Math.random() * gridArr.value[0].length).toString();
  const gridBlock = gridArr.value[randomPosRow][randomPosCol];
  snakeArr.value.push({
    top: (gridBlock.blockInfo.top - gridBorder.value.top) + 1,
    left: (gridBlock.blockInfo.left - gridBorder.value.left) + 1
  });
  snakeArr.value.push({
    top: ((gridBlock.blockInfo.top + 50) - gridBorder.value.top) + 1,
    left: (gridBlock.blockInfo.left - gridBorder.value.left) + 1
  });
  snakeArr.value.push({
    top: ((gridBlock.blockInfo.top + 100) - gridBorder.value.top) + 1,
    left: (gridBlock.blockInfo.left - gridBorder.value.left) + 1
  });
  snakeArr.value.push({
    top: ((gridBlock.blockInfo.top + 150) - gridBorder.value.top) + 1,
    left: (gridBlock.blockInfo.left - gridBorder.value.left) + 1
  });
  snakeArr.value.push({
    top: ((gridBlock.blockInfo.top + 200) - gridBorder.value.top) + 1,
    left: (gridBlock.blockInfo.left - gridBorder.value.left) + 1
  });
}

onMounted(() => {
  document.addEventListener('keydown', getKeyPress);

  // Build Grid
  for (let i = 0; i < 10; i++) {
    gridArr.value.push([]);
    for (let k = 0; k < 10; k++) {
      gridArr.value[i].push({
        blockCoords: `${i}${k}`,
        blockInfo: null
      });
    }
  }

  // Get Grid Info
  setTimeout(() => {
    gridBorder.value = document.getElementById('gridBorder').getBoundingClientRect();
    gridArr.value.forEach(row => {
      row.forEach(col => {
        col.blockInfo = document.getElementById(`${col.blockCoords}`).getBoundingClientRect();
      })
    })
    chooseSnakeStartingHeadPositionOnGrid();
  }, 500)
})

</script>

<style scoped>
.b1 {
  border: 1px solid deeppink;
}
.b2 {
  border: 1px solid yellow;
}
.b3 {
  border: 1px solid deepskyblue;
}
.b4 {
  border: 1px solid green;
}
</style>