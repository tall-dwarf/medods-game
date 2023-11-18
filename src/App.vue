<template>
  <div class="app" id="app">
    <GameSimon />
  </div>
</template>

<script>
import GameSimon from '@/components/GameSimon.vue';

export default {
  name: 'App',
  components: {
    GameSimon
  }
}
</script>

<style>
.app{
  display: flex;
  align-items: center;
  justify-content: center;
}
.aqua {
  background-color: aqua;
}

.red {
  background-color: red;
}

.yellow {
  background-color: yellow;
}

.green {
  background-color: green;
}
</style>
