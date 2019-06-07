<template>
  <div>
    <section class="row controls" v-if="!isRunning">
      <div class="small-12 columns">
        <button id="start-game" @click="onUpdateGameStatus(true)">
          START NEW GAME
        </button>
      </div>
    </section>
    <section class="row controls" v-else>
      <div class="small-12 columns">
        <button
          id="attack"
          :disabled="parseInt(opponentHealth) < 0"
          @click="userMove('attack')"
        >
          PUNCH
        </button>
        <button
          id="special-attack"
          :disabled="parseInt(opponentHealth) < 1"
          @click="userMove('specialAttack')"
        >
          KICK
        </button>
        <button
          id="heal"
          :disabled="parseInt(playerHealth) < 1"
          @click="userMove('heal')"
        >
          HEAL
        </button>
        <button id="give-up" @click="onUpdateGameStatus(false)">GIVE UP</button>
      </div>
    </section>
    <section class="row log" v-if="gameLog.length > 0">
      <div class="small-12 columns">
        <ul>
          <li :key="index" v-for="(log, index) in gameLog" class="row" :style="styling(log.user)">
            <p>{{ log.msg }}</p>
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Actions',
  props: ['is-running', 'player-health', 'opponent-health'],
  data: () => ({
    gameLog: [],
  }),
  methods: {
    checkWinner(user, damageTaken) {
      if (
        user === 'opponent' &&
        this.$props.opponentHealth - damageTaken <= 0
      ) {
        setTimeout(() => {
          alert('You win! Opponent is dead. \nGame over!');
        }, 600);
        this.actionLog(
          `You defeated the opponent - awesome! Game over!`,
          'player',
        );
        this.onUpdateGameStatus(false);
      }
      if (user === 'player' && this.$props.playerHealth - damageTaken <= 0) {
        setTimeout(() => {
          alert('You lose! Opponent beats you in the fight. \nGame over!');
        }, 600);
        this.actionLog(
          `Opponent wins as you fall to a crushing defeat! Game Over!`,
          'opponent',
        );
        this.onUpdateGameStatus(false);
      }
    },
    onUpdateGameStatus(status) {
      this.$emit('onUpdateGameStatus', status);
      if (status) {
        this.gameLog = [];
      }
    },
    actionLog(msg, user) {
      if (this.$props.isRunning) {
        this.gameLog.splice(0, 0, { msg, user });
      }
    },
    heal(char, min, max) {
      const healAmount = Math.max(Math.floor(Math.random() * max) + 1, min);
      this.$emit('onHealUser', char, healAmount);
      if (char === 'player') {
        this.actionLog(
          `You healed to increase your health by ${healAmount}`,
          char,
        );
      } else {
        this.actionLog(
          `Opponent took time to heal themselves by ${healAmount}`,
          char,
        );
      }
    },
    damage(min, max, user, move) {
      const damage = Math.max(Math.floor(Math.random() * max) + 1, min);
      if (user === 'player' && this.$props.playerHealth >= 0) {
        this.$emit('onUpdateHealth', 'opponent', damage);
        this.actionLog(
          `User ${move} opponent resulting in damage of ${damage}`,
          user,
        );
        this.checkWinner('opponent', damage);
      } else if (this.$props.opponentHealth >= 0) {
        this.$emit('onUpdateHealth', 'player', damage);
        this.actionLog(
          `Opponent retaliated causing you damage of ${damage}`,
          user,
        );
        this.checkWinner('player', damage);
      }
    },
    styling(idx) {
      if (idx === 'player') {
        return {
          backgroundColor: '#34495e',
        };
      } else {
        return {
          backgroundColor: '#41b883',
        };
      }
    },
    async opponentMove(min, max) {
      const responseTime = Math.max(Math.floor(Math.random() * max) + 1, min);
      await setTimeout(() => {
        this.damage(5, 17, 'opponent');
      }, responseTime);
    },
    async userMove(move) {
      if (this.$props.playerHealth > 0) {
        switch (move) {
          case 'attack':
            this.damage(3, 10, 'player', 'punched');
            await this.opponentMove(300, 1000);
            break;
          case 'specialAttack':
            this.damage(6, 20, 'player', 'kicked');
            await this.opponentMove(500, 1500);
            break;
          case 'heal':
            this.heal('player', 4, 16);
            setTimeout(() => {
              this.heal('opponent', 4, 16);
            }, 600);
            break;
          default:
            return;
        }
      }
    },
  },
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
ul > li > p {
  color: #fff;
}

ul {
  padding: 0;
}

ul > li {
  padding: 5px;
  width: auto;
  list-style: none;
  margin-bottom: 10px;

}

.row.log {
  height: 60vh;
  width: auto;
  overflow-y: scroll;
}
</style>
