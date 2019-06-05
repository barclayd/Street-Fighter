<template>
  <div>
    <section class="row controls" v-if="!isRunning">
      <div class="small-12 columns">
        <button id="start-game" @click="onUpdateGameStatus(true)">START NEW GAME</button>
      </div>
    </section>
    <section class="row controls" v-else>
      <div class="small-12 columns">
        <button id="attack" :disabled="parseInt(opponentHealth) === 0" @click="userMove('attack')">PUNCH</button>
        <button id="special-attack" @click="userMove('specialAttack')">KICK</button>
        <button id="heal" @click="userMove('heal')">HEAL</button>
        <button id="give-up" @click="onUpdateGameStatus(false)">GIVE UP</button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Actions',
  props: ['is-running', 'player-health', 'opponent-health'],
  methods: {
    onUpdateGameStatus(status) {
      this.$emit('onUpdateGameStatus', status);
    },
    heal(char, min, max) {
      const healAmount = Math.max(Math.floor(Math.random() * max) + 1, min);
      this.$emit('onHealUser', char, healAmount)
    },
    damage(min, max, user) {
      const damage = Math.max(Math.floor(Math.random() * max) + 1, min);
      if (user === 'player') {
        this.$emit('onUpdateHealth', 'opponent', damage);
      } else {
        this.$emit('onUpdateHealth', 'player', damage);
      }
    },
    userMove(move) {
      switch (move) {
        case 'attack':
          this.damage(3, 10, 'player');
          break;
        case 'specialAttack':
          this.damage(6, 20, 'player');
          break;
        case 'heal':
          this.heal('player', 4, 16);
          break;
        default:
          return;
      }
    }
  }
};
</script>

<style scoped>
button {
  font-size: 20px;
  background-color: #eee;
  padding: 12px;
  box-shadow: 0 1px 1px black;
  margin: 10px;
  outline: none;
}
</style>
