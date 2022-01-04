<template>
  <div class="section">
    <div class="box has-background-warning">
      <h1 class="title is-2 has-text-centered">GOTCHA</h1>
      <h2 class="title is-3 has-text-centered">Have you got what it takes to survive?</h2>
    </div>

    <div class="columns has-text-centered my-6">
      <div class="column">
        <h3 class="title is-3 ">Target</h3>
        <img :src="targetImage">
        <h3 class="title is-3">{{ target }}</h3>
      </div>
      <div class="column">
        <h3 class="title is-3">Weapon</h3>
        <img :src="weaponImage">
        <h3 class="title is-3">{{ weapon }}</h3>
      </div>
      <div class="column">
        <h3 class="title is-3">Room</h3>
        <span v-if="room">
          <h3 class="title is-3 mt-6">{{ room }}</h3>
        </span>
        <span v-else>
          <img src="/undefined.jpg">
        </span>
      </div>
    </div>

    <div class="has-text-centered">
      <input type="text" v-model="input" class="input" style="width: 50%;" placeholder="Type your name" autofocus>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Gotcha',
  data: function () {
    return {
      seed: 1, // Change this for every new game
      players: [
        'jane',
        'joe',
        'dick',
        'harry'
      ],
      weapons: [
        'candlestick',
        'knife',
        'revolver',
        'rope'
      ],
      rooms: [
        'ballroom',
        'kitchen',
        'library',
        'lounge'
      ],
      dataset: {},
      input: ''
    }
  },
  mounted: function () {
    const shuffledPlayers = this.shuffle(this.players, this.seed)
    for (let index = 0; index < shuffledPlayers.length; index++) {
      const player = shuffledPlayers[index]
      const target = shuffledPlayers[index+1] || shuffledPlayers[0]
      const weapon = this.weapons[index]
      const room = this.rooms[index]
      this.dataset[player] = { target: target, weapon: weapon, room: room }
    }
  },
  computed: {
    player: function () {
      return this.input.toLowerCase()
    },
    target: function () {
      return this.dataset[this.player]?.target
    },
    weapon: function () {
      return this.dataset[this.player]?.weapon
    },
    room: function () {
      return this.dataset[this.player]?.room
    },
    targetImage: function () {
      return `/${this.target}.jpg`
    },
    weaponImage: function () {
      return `/${this.weapon}.jpg`
    }
  },
  methods: {
    shuffle: function(array, seed) {
      let currentIndex = array.length, temporaryValue, randomIndex;
      seed = seed || 1;
      let random = function() {
        var x = Math.sin(seed++) * 10000;
        return x - Math.floor(x);
      };
      while (0 !== currentIndex) {
        randomIndex = Math.floor(random() * currentIndex);
        currentIndex -= 1;
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }
      return array;
    }
  }
}
</script>

<style>
img {
  width: 300px;
  border: 1px solid grey;
  padding: 20px;
  border-radius: 10px;
}
</style>
