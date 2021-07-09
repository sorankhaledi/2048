<template>
  <div class="grid-system">
    <Culomn v-for="(row, i) in rows" :key="i" :value="row" :num="i" />
  </div>
</template>

<script>
import Culomn from "./Column.vue";

export default {
  data() {
    return {
    };
  },
  props: ['rows'],
  components: {
    Culomn,
  },
};
</script>

<style scoped>
.grid-system {
  display: grid;
  grid-template-rows: repeat(4, 60px);
  grid-template-columns: repeat(4, 60px);
  border-radius: 5px;
  background-color: #f0efeb;
}
</style>
