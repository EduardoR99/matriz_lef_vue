<template>
  <div>
    <table>
      <tr v-for="(row, rowIndex) in matrixItems" :key="rowIndex">
        <td v-for="(item, columnIndex) in row" :key="columnIndex" :class="{'same-origin-destination': isSameOriginDestination(rowIndex, columnIndex)}">
          <input type="text" v-model="matrixItems[rowIndex][columnIndex]" />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
import jsonData from '@/data.json';

export default {
  data() {
    return {
      matrixItems: [],
      matrixSize: 0,
    };
  },
  created() {
    this.loadMatrixItems();
  },
  methods: {
    loadMatrixItems() {
      const items = jsonData.jsonData;

      this.matrixSize = Math.max(
        ...items.map(item => item.origem),
        ...items.map(item => item.destino)
      );

      this.matrixItems = Array.from(
        { length: this.matrixSize },
        () => Array(this.matrixSize).fill(0)
      );

      items.forEach(item => {
        const { origem, destino, aliquota } = item;
        this.matrixItems[origem - 1][destino - 1] = aliquota;
      });
    },
    isSameOriginDestination(rowIndex, columnIndex) {
      const origem = rowIndex + 1;
      const destino = columnIndex + 1;
      return origem === destino;
    },
  },
};
</script>

<style>
input{
  margin: 0;
  padding: 0;
  width: 1.5rem;
  height: 1.5rem;
}
.same-origin-destination {
  background-color: #F00;
}
</style>
