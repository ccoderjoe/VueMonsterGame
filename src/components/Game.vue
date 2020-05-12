<template>
  <div class="container">
    <section class="players d-flex my-4">
      <div class="player">
        <h4 class="player-name text-center">YOU</h4>
        <div class="health-bar">
          <p class="health">{{ playerHealth }}</p>
          <div class="health-remaining" :style="{ width: playerHealth + '%'}"></div>
          <div class="health-empty"></div>
        </div>
      </div>
      <div class="player">
        <h4 class="player-name text-center">MONSTER</h4>
        <div class="health-bar">
          <p class="health">{{ monsterHealth }}</p>
          <div class="health-remaining" :style="{ width: monsterHealth + '%' }"></div>
          <div class="health-empty"></div>
        </div>
      </div>
    </section>
    <div>
      <b-card class="mb-2 text-center d-flex align-center" v-if="gameIsRunning">
        <b-button href="#" variant="danger mr-3" @click="basicAttack">Attack</b-button>
        <b-button href="#" variant="warning mr-3" @click="specialAttack">Special Attack</b-button>
        <b-button href="#" variant="success mr-3" @click="heal">Heal</b-button>
        <b-button href="#" variant="secondary mr-3" @click="giveUp">Give Up</b-button>

        <div v-if="playerHealth <= 0 || monsterHealth <= 0 ? endGame() : ''"></div>
      </b-card>
      <b-card class="mb-2 text-center d-flex align-center" v-else>
        <b-button
          href="#"
          variant="success mr-3"
          @click="{{gameIsRunning = true
        playerHealth = 100;
        monsterHealth = 100;
        turns = [];
        heals = [];
        }}"
        >Start new game</b-button>
      </b-card>
      <b-card class="mb-2 text-center d-flex">
        <div>
          <ul>
            <li
              v-for="turn in turns"
              :key="turn.playerName"
              :class="[turn.attacker === 'Player' ?  'color-player' : 'color-monster']"
            >{{ turn.attacker }} {{ turn.target }} for {{ turn.action }}</li>
          </ul>
        </div>
      </b-card>
      <div v-if="!playerTurn ? monsterAttack() : ''"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      turns: [],
      gameIsRunning: false,
      playerTurn: true
    };
  },
  methods: {
    basicAttack: function() {
      let attack = Math.ceil(Math.random() * 7);
      this.monsterHealth -= attack;
      this.turns.unshift({
        attacker: "Player",
        target: "Monster",
        action: attack
      });
      this.playerTurn = false;
    },
    monsterAttack: function() {
      let attack = Math.ceil(Math.random() * 12);
      this.playerHealth -= attack;
      this.turns.unshift({
        attacker: "Monster",
        target: "Player",
        action: attack
      });
      this.playerTurn = true;
    },
    specialAttack: function() {
      let attack = Math.ceil(Math.random() * 7 + 7);
      this.monsterHealth -= attack;
      this.turns.unshift({
        attacker: "Player",
        target: "attacks Monster",
        action: attack
      });
      this.playerTurn = false;
    },
    heal: function() {
      let heal = Math.ceil(Math.random() * 10);
      this.playerHealth += heal;
      this.turns.unshift({
        attacker: "Player",
        target: "heals self",
        action: heal
      });
      this.playerTurn = false;
    },
    giveUp: function() {
      this.playerHealth = 0;
    },
    endGame: function() {
      if (this.monsterHealth <= 0) {
        window.alert("Game ends! Congrats you won!");
      } else {
        window.alert("Game ends! Seems monster beated you!");
      }
      this.gameIsRunning = false;
    }
  }
};
</script>



<style scoped>
.container {
  max-width: 900px;
  width: 90%;
}
.players {
  justify-content: space-between;
}

.player {
  width: 45%;
}

.health-bar {
  position: relative;

  height: 40px;
  background-color: grey;
}
.health {
  position: relative;
  z-index: 99999;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.health-empty {
  height: 40px;
  background-color: transparent;
}

.health-remaining {
  background-color: green;
  height: 40px;
  width: 100%;

  position: absolute;
}

.color-player {
  background-color: lightgreen;
  margin-top: 5px;
  margin-bottom: 5px;
}

.color-monster {
  background-color: rgb(228, 99, 99);
  margin-top: 5px;
  margin-bottom: 5px;
}
</style>