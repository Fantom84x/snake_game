<template>
  <div
    class="d-flex flex-column justify-center align-center"
    style="width: 100%; height: 100%; position: relative"
  >

    <div id="gridBorder">
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
          :style="`top: ${snakeBlock.top + 1}px; left: ${snakeBlock.left + 1}px`"
        ></div>
      </template>

    </div>

    <div
      v-if="applePosition"
      style="position: absolute; width: 48px; height: 48px; background-color: rgba(128, 0 , 0, 0.75)"
      :style="`top: ${applePosition.blockInfo.top + 1}px; left: ${applePosition.blockInfo.left + 1}px`"
    ></div>
  </div>

</template>

<script setup>
import {onMounted, ref} from "vue";

const gridArr = ref([]);
const snakeArr = ref([]);
const gridBorder = ref(null);
const setTimeoutVal = ref(null);
const previousPressedKeyCode = ref(null);

const applePosition = ref(null);

function getKeyPress(event) {

  if (previousPressedKeyCode.value === event.keyCode)
    return;

  previousPressedKeyCode.value = event.keyCode;

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

  if (setTimeoutVal.value)
    clearTimeout(setTimeoutVal.value);

  const newSnakeArr = [];

  for (let i = 0; i < snakeArr.value.length; i++) {
    if (i === 0) {

      const newSnakeHeadTopValue = snakeArr.value[i].top + topAdjustmentVal;
      const newSnakeHeadLeftValue = snakeArr.value[i].left + leftAdjustmentVal;

      const newGridBlock = gridArr.value.map(row => {
        return row.find(gridBlock => {
          if (gridBlock.blockInfo.top === newSnakeHeadTopValue && gridBlock.blockInfo.left === newSnakeHeadLeftValue)
            return true;
        })
      })
        .filter(item => item)
        .pop();

      if (!newGridBlock) {
        console.log('CRASHED INTO A WALL')
        return;
      }

      newSnakeArr.push({
        top: newSnakeHeadTopValue,
        left: newSnakeHeadLeftValue,
        blockCoords: newGridBlock.blockCoords
      });
      continue;
    }

    newSnakeArr.push({
      top: snakeArr.value[i - 1].top,
      left: snakeArr.value[i - 1].left,
      blockCoords: snakeArr.value[i - 1].blockCoords
    });

  }

  console.log('newSnakeArr', newSnakeArr);

  snakeArr.value = newSnakeArr;

  setTimeoutVal.value = setTimeout(() => {
    moveSnake(topAdjustmentVal, leftAdjustmentVal);
  }, 500)

}

function spawnAppleInRandomPosition() {
  applePosition.value = getRandomSpawnPositionBlock();
  let isAppleOnSnake = snakeArr.value.find(block => block.coords === applePosition.value.coords);
  while (applePosition.value.coords === isAppleOnSnake) {
    applePosition.value = getRandomSpawnPositionBlock();
    isAppleOnSnake = snakeArr.value.find(block => block.coords === applePosition.value.coords);
  }
}

function getRandomSpawnPositionBlock() {
  const randomPosRow = Math.floor(Math.random() * gridArr.value.length).toString();
  const randomPosCol = Math.floor(Math.random() * gridArr.value[0].length).toString();
  return gridArr.value[randomPosRow][randomPosCol];
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

    const randomSpawnPosition = getRandomSpawnPositionBlock();
    snakeArr.value.push({
      top: randomSpawnPosition.blockInfo.top,
      left: randomSpawnPosition.blockInfo.left,
      blockCoords: randomSpawnPosition.blockCoords
    });

    spawnAppleInRandomPosition();

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