<template>
  <div class="bg-blue-400 h-screen text-white">
    <div class="flex h-full items-center justify-center flex-col">
    <h1 class="text-white text-4xl pt-4">OP GAME</h1>
    <span class="flex justify-center items-center">
      <p class="shadow-md my-5 max-w-sm p-4">
        <strong>What do you do in this game?</strong><br />
        You play against an AI or an other player. You have 12 pens, from which
        you can take one or two pencils away every round. The player which takes
        the pencils as last, wins. <br />
        Have fun!
      </p>
    </span>
    <div class="flex justify-center items-center my-4">
      <label>
        <span>Play against AI?</span>
      </label>
      <input
        class="
        w-6 h-6 mx-2
        rounded
        text-green-500
        focus:ring-white
        focus:ring-opacity-50
        border-white  
        border-2
        "
        type="checkbox"
      />
    </div>
    <button class="button">Start Game</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.button {
  @apply border-green-400 text-white bg-green-400 border-2 rounded-lg w-48 h-12 z-10;
}
.button:active {
  @apply ring-2 ring-white;
}

.checkbox {
  @apply w-6 h-6 bg-red-400 text-white border-white;
}
</style>
