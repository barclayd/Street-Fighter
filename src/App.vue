<template>
  <div id="app">
    <healthbars
      :player-health="playerHealth"
      :opponent-health="opponentHealth"
    />
    <actions
      :is-running="isRunning"
      @onUpdateGameStatus="onUpdateGameStatus"
      @onUpdateHealth="onUpdateHealth"
      @onHealUser="onHealUser"
      :player-health="playerHealth"
      :opponent-health="opponentHealth"
    />
    <actionLog />
  </div>
</template>

<script>
import Healthbars from './components/Healthbars';
import Actions from './components/Actions';
import ActionLog from './components/ActionLog';
import './styles/app.css';
import './styles/foundation.min.css';

export default {
  name: 'app',
  components: {
    Healthbars,
    Actions,
    ActionLog,
  },
  data() {
    return {
      isRunning: false,
      playerHealth: 80,
      opponentHealth: 100,
    };
  },
  methods: {
    onUpdateGameStatus(status) {
      this.isRunning = status;
    },
    onUpdateHealth(char, damage) {
      if (char === 'player' && this.playerHealth > 0) {
        this.playerHealth -= damage;
        if (this.playerHealth < 0) {
          this.playerHealth = 0;
        }
      } else if (char === 'opponent' && this.opponentHealth > 0) {
        this.opponentHealth -= damage;
        if (this.opponentHealth < 0) {
          this.opponentHealth = 0;
        }
      }
    },
    onHealUser(char, healAmount) {
      if (char === 'player') {
        this.playerHealth += healAmount;
        if (this.playerHealth > 100) {
          this.playerHealth = 100;
        }
      } else if (char === 'opponent') {
        this.opponentHealth += healAmount;
        if (this.opponentHealth > 100) {
          this.opponentHealth = 100;
        }
      }
    }
  },
};
</script>

<style></style>
