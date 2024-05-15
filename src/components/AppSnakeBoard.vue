<template>
  <div
    class="b1 d-flex flex-column"
    style="width: 100%; height: 100%"
  >
    <div
      v-for="(row, i) in cellsInfoArr"
      :key="i + 'A'"
      class="d-flex"
    >
      <div
        v-for="(cell, k) in row"
        :key="k + 'A'"
        :id="cell"
        class="d-flex"
      >
        <div
          class="b2"
          style="width: 30px; height: 30px"
        ></div>

      </div>

    </div>

    <v-btn
      text="Click Me"
      @click="myFunction"
    ></v-btn>

  </div>
</template>

<script setup>
import {onMounted, ref} from "vue";

const cellsInfoArr = ref([]);

function myFunction() {
  console.log(document.getElementById('43').getBoundingClientRect());
}

onMounted(() => {
  for (let i = 0; i < 5; i++) {
    cellsInfoArr.value.push([]);
    for (let j = 0; j < 5; j++) {
      cellsInfoArr.value[i].push(`${i}${j}`);
    }
  }

  console.log(cellsInfoArr.value);

})

</script>

<style scoped>
.b1 {
  border: 1px solid deeppink;
}
.b2 {
  border: 1px solid deepskyblue;
}
.b3 {
  border: 1px solid yellow;
}
</style>