<template>
  <div class="grid-cell" :class="{ alive: isAlive }" @click="toggleCell(X, Y)">
    <span></span>
  </div>
</template>

<script>
// this is the component 'options' object
export default {
  name: 'Cell',
  // data() {
  // return {
  //   cellAlive: isAlive,
  // };
  // },
  props: {
    X: Number,
    Y: Number,
    isAlive: Number,
  },
  methods: {
    toggleCell(X, Y) {
      // console.log(`X: ${X} Y: ${Y}`);
      this.$emit('toggle-cell', X, Y);
    },
  },
  computed: {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* #grid-cell {
  border: 1px solid black;
} */
span {
  display: flex;
  border: 1px solid black;
  width: 2rem;
  height: 2rem;
  /* background-color: rgb(0, 201, 140); */
}

.alive {
  background-color: rgb(0, 201, 140);
}
</style>
