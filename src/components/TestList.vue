<template>
  <div>
    <canvas ref="canvas" width="1500" height="900" style="background-color: #ffffff"></canvas>
    <button @click="addTriangle()">Треугольник</button>
  </div>
</template>

<script>
import { fabric } from "fabric";

export default {
  name: 'TestList',
  mounted() {
    this.canvas = new fabric.Canvas(this.$refs.canvas)
  },
  methods: {
    addTriangle() {
      var triangle = new fabric.Triangle({
        width: 200,
        height: 200,
        fill: 'red  ',
        left: 100,
        top: 100
      })
      this.canvas.add(triangle)
    }
  }
}
</script>

<style>
/* Ваши стили */
</style>
