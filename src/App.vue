<template>
  <div class="bg-blue-400 h-screen text-white">
    <span class="absolute bottom-0 right-0 m-4">
      <p>
        Made by
        <a href="https://github.com/Criepstar" target="about_blank"
          >Criepstar</a
        >
      </p>
    </span>
    <div
      v-if="route == 'home'"
      class="flex h-full items-center justify-center flex-col"
    >
      <h1 class="text-white text-4xl pt-4">UNNAMED PEN GAME</h1>
      <span class="flex justify-center items-center">
        <p
          class="shadow-md my-5 mx-6 sm:max-w-sm p-4 bg-white text-black rounded-md"
        >
          <strong>How does the game work?</strong><br />
          At the start of the game there are 12 pencils. 
          Every round you can take 1 or 2 pencils away. 
          The winner is the one who takes the last pencil!  <br />
          Have fun!
        </p>
      </span>
      <div class="flex justify-center items-center my-4">
        <label>
          <span>Play against Computer?</span>
        </label>
        <input
          class="checkbox w-6 h-6 mx-2 rounded text-green-400 focus:ring-white focus:ring-opacity-50 border-white border-0"
          type="checkbox"
          v-model="ai"
        />
      </div>
      <button class="button flex justify-center items-center" @click="start"><span><img class="mr-2" src="./assets/play.svg"></span>Start Game</button>
    </div>
    <div
      v-if="route == 'game'"
      class="flex flex-col justify-center items-center h-full"
    >
      <h1 class="text-white text-4xl pt-3 mb-6">UNNAMED PEN GAME</h1>
      <p v-if="pens.length > 0 && !ai" class="text-lg font-bold">
        Player {{ current }} is on the turn
      </p>
      <p v-if="pens.length > 0 && ai && !turn" class="text-lg font-bold">
        Player is on the turn
      </p>
      <p v-if="pens.length > 0 && ai && turn" class="text-lg font-bold">
        Computer is on the turn
      </p>
      <div
        v-if="pens.length > 0"
        class="flex items-center mt-2 justify-center flex-row"
      >
        <span v-for="(pen, i) in pens" v-bind:key="i">
          <img class="w-5 sm:w-11 mx-1" src="./assets/pen.svg" />
        </span>
        
      </div>
      <div class="w-12 h-12 relative bg-red-200"></div>
      <div v-if="pens.length > 0" class="mt-8">
        How many pens do you want to take? <br />
        <button class="game-button" @click="takeOne" :disabled="turn">1</button>
        <button class="game-button" @click="takeTwo" :disabled="turn">2</button>
      </div>
      <div v-if="pens.length == 0">
        <p v-if="!ai" class="shadow-md bg-white text-black rounded p-5">
          The game is over! <br />
          Player {{ current }} won! <br /><br />
          Player 1: {{ scoreP1 }}<br />
          Player 2: {{ scoreP2 }}
        </p>
        <p v-if="ai && !turn" class="shadow-md bg-white text-black rounded p-5">
          The game is over! <br />
          Player won! <br /><br />
          Player: {{ scoreP1 }}<br />
          Computer: {{ scoreP2 }}
        </p>
        <p v-if="ai && turn" class="shadow-md bg-white text-black rounded p-5">
          The game is over! <br />
          Computer won! <br /><br />
          Player: {{ scoreP1 }}<br />
          Computer: {{ scoreP2 }}
        </p>
        <div class="flex justify-center items-center mt-4">
          <label>
            <span>Play against Computer?</span>
          </label>
          <input
            class="checkbox w-6 h-6 mx-2 rounded text-green-400 focus:ring-white focus:ring-opacity-50 border-white border-0"
            type="checkbox"
            v-model="ai_rs"
          />
        </div>
        <button class="button mt-2 flex justify-center items-center" @click="reset"><span><img class="mr-2" src="./assets/repeat.svg"></span>Play again</button>
      </div>
    </div>
    <loading v-if="loading"></loading>
  </div>
</template>

<script>
import loading from "./components/loading.vue";
export default {
  components: { loading },
  name: "App",
  data() {
    return {
      loading: false,
      ai: false,
      ai_rs: false,
      pens: [
        {
          id: 1,
        },
        {
          id: 2,
        },
        {
          id: 3,
        },
        {
          id: 4,
        },
        {
          id: 5,
        },
        {
          id: 6,
        },
        {
          id: 7,
        },
        {
          id: 8,
        },
        {
          id: 9,
        },
        {
          id: 10,
        },
        {
          id: 11,
        },
        {
          id: 12,
        },
      ],
      route: "home",
      turn: false,
      current: 1,
      scoreP1: 0,
      scoreP2: 0,
    };
  },
  watch: {
    ai: function () {
      this.scoreP1 = 0;
      this.scoreP2 = 0;
    },
    "pens.length": function () {
      if (this.pens.length == 0) {
        if (this.ai) {
          if (this.turn) {
            this.scoreP2 += 1;
          } else {
            this.scoreP1 += 1;
          }
        } else {
          if (this.current == 1) {
            this.scoreP1 += 1;
          } else if (this.current == 2) {
            this.scoreP2 += 1;
          }
        }
      }
    },
  },
  methods: {
    start() {
      this.loading = true;
      this.ai_rs = this.ai;
      const starter = Math.floor(Math.random() * 2 + 1);
      if (!this.ai) {
        this.current = starter;
      } else {
        if (starter == 1) {
          this.turn = false;
        } else if (starter == 2) {
          this.turn = true;
        }
      }
      setTimeout(() => {
        this.loading = false;
        this.route = "game";
        if (this.turn) {
          this.aiPlay();
        }
      }, 1000);
    },
    takeOne() {
      this.pens.splice(0, 1);
      this.setCurrent();
    },
    takeTwo() {
      this.pens.splice(0, 2);
      this.setCurrent();
    },
    checkWinner() {},
    setCurrent() {
      if (this.pens.length > 0) {
        if (!this.ai) {
          if (this.current == 1) {
            this.current = 2;
          } else {
            this.current = 1;
          }
        } else {
          if (this.pens.length > 0) {
            this.turn = true;
            this.aiPlay();
          }
        }
      }
    },
    aiPlay() {
      let length = this.pens.length;
      setTimeout(() => {
        if (length >= 9) {
          const random = Math.floor(Math.random() * 2);
          if (random == 0) {
            this.pens.splice(0, 1);
          } else {
            this.pens.splice(0, 2);
          }
        } else if (length == 8) {
          this.pens.splice(0, 2);
        } else if (length == 7) {
          const random = Math.floor(Math.random() * 2);
          if (random == 0) {
            this.pens.splice(0, 1);
          } else {
            this.pens.splice(0, 2);
          }
        } else if (length == 6) {
          const random = Math.floor(Math.random() * 2);
          if (random == 0) {
            this.pens.splice(0, 1);
          } else {
            this.pens.splice(0, 2);
          }
        } else if (length == 5) {
          this.pens.splice(0, 2);
        } else if (length == 4) {
          this.pens.splice(0, 1);
        } else if (length == 3) {
          const random = Math.floor(Math.random() * 2);
          if (random == 0) {
            this.pens.splice(0, 1);
          } else {
            this.pens.splice(0, 2);
          }
        } else if (length == 2) {
          this.pens.splice(0, 2);
        } else if (length == 1) {
          this.pens.splice(0, 1);
        }
        if (this.pens.length > 0) {
          this.turn = false;
        }
      }, 500);
    },
    reset() {
      this.loading = true;
      let starter = Math.floor(Math.random() * 2 + 1);
      setTimeout(() => {
        this.loading = false;
        this.turn = false;
        this.ai = this.ai_rs;
        if (!this.ai) {
          this.current = starter;
        } else {
          if (starter == 1) {
            this.turn = false;
          } else if (starter == 2) {
            this.turn = true;
          }
        }
        this.pens = [
          {
            id: 1,
          },
          {
            id: 2,
          },
          {
            id: 3,
          },
          {
            id: 4,
          },
          {
            id: 5,
          },
          {
            id: 6,
          },
          {
            id: 7,
          },
          {
            id: 8,
          },
          {
            id: 9,
          },
          {
            id: 10,
          },
          {
            id: 11,
          },
          {
            id: 12,
          },
        ];
        if(this.turn){
          this.aiPlay()
        }
      }, 1000);
    },
  },
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
  @apply border-green-400 text-white bg-green-400 border-2 rounded-lg w-48 h-12;
}
.button:active {
  @apply ring-2 ring-white;
}

.checkbox:checked {
  @apply border-green-400;
}
.game-button {
  @apply bg-green-400 text-white m-3 w-12 h-12 rounded-xl;
}
.game-button:active {
  @apply ring-2 ring-white;
}
.game-button:disabled {
  @apply bg-opacity-50;
}
@keyframes moveup{
  from {bottom:0px}
  to{top:50%}
}
</style>
