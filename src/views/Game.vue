<template>
  <v-container fluid>
    <v-row>
      <v-col>
        <v-text-field
          v-model.number="formData.sizeX"
          label="Size X"
          type="number"
          min="1"
          max="100"
          @input="validateField('sizeX', 1, 100)"
          @change="drawGrid"
        ></v-text-field>
      </v-col>
      <v-col>
        <v-text-field
          v-model.number="formData.sizeY"
          label="Size Y"
          type="number"
          min="1"
          max="100"
          @input="validateField('sizeY', 1, 100)"
          @change="drawGrid"
        ></v-text-field>
      </v-col>
    </v-row>
    <div class="grid-container">
      <div v-for="(row, rowIndex) in grid" :key="rowIndex" class="grid-row">
        <div
          v-for="(col, colIndex) in row"
          :key="colIndex"
          class="grid-item"
          :style="{ backgroundColor: col ? 'blue' : 'white' }"
          @mouseover="toggleColor(rowIndex, colIndex)"
        ></div>
      </div>
    </div>
  </v-container>
</template>
  
  <script setup>
import { ref, reactive } from "vue";

const formData = reactive({
  sizeX: 64,
  sizeY: 64,
});

const validateField = (field, min, max) => {
  if (formData[field] > max) {
    formData[field] = max;
  } else if (formData[field] < min) {
    formData[field] = min;
  }
};
const grid = reactive([]);

const drawGrid = () => {
  grid.splice(0, grid.length);
  for (let i = 0; i < formData.sizeY; i++) {
    grid.push(new Array(formData.sizeX).fill(false));
  }
};

const toggleColor = (row, col) => {
  grid[row][col] = !grid[row][col];
};

drawGrid();
</script>
  
  <style scoped>
.grid-container {
  display: flex;
  flex-direction: column;
}

.grid-row {
  display: flex;
}

.grid-item {
  width: 36px;
  height: 36px;
  border: 1px solid #333;
}
</style>
  