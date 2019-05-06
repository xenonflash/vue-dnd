<template>
  <dnd>
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
  </dnd>
</template>
<script>
import Dnd from "./dnd.vue";

export default {
  name: "demo",
  data() {
    return {};
  },
  components: {
    Dnd
  },
  computed: {},
  mounted() {
    this.init();
  },
  methods: {
    init() {}
  },
  watch: {}
};
</script>
<style scoped>
  .item{
    width: 200px;
    border: 1px solid;
    padding: 10px;
    margin: 10px;
  }
</style>