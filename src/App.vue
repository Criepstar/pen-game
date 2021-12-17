<template>
  <div class="bg-blue-400 h-screen">
    <h1 class="text-white text-4xl pt-4">OP GAME</h1>
    <button class="button">Start Game</button>
  </div>
</template>

<script>

export default {
  name: 'App',
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.button{
  @apply border-green-400 bg-green-400 border-2 rounded-lg w-48 h-12 z-10; 
}
.button:active{
  @apply ring-2 ring-white
}
</style>
