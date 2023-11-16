<template>
  <div id="app">
    <PedidoList />
  </div>
</template>

<script>
import PedidoList from "@/views/PedidoList.vue";

export default {
  name: "App",
  components: {
    PedidoList,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  /*margin-top:60px;*/
}
</style>