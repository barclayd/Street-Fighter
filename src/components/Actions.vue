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
    <section class="row log">
      <div class="small-12 columns">
        <ul v-for="(log, index) in gameLog">
          <li class="row" :style="styling(index)">
            <p>{{ log }}</p>
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
          alert('You win! Opponent is dead. \n Game over!');
        }, 600);
        this.onUpdateGameStatus(false);
      }
      if (user === 'player' && this.$props.playerHealth - damageTaken <= 0) {
        setTimeout(() => {
          alert('You lose! Opponent is the winner. \n Game over!');
        }, 600);
        this.onUpdateGameStatus(false);
      }
    },
    onUpdateGameStatus(status) {
      this.$emit('onUpdateGameStatus', status);
    },
    heal(char, min, max) {
      const healAmount = Math.max(Math.floor(Math.random() * max) + 1, min);
      this.$emit('onHealUser', char, healAmount);
      if (char === 'player') {
        this.gameLog.splice(0, 0,
          `You healed to increase your health by ${healAmount}`,
        );
      } else {
        this.gameLog.splice(0, 0,
          `Opponent took time to heal themselves by ${healAmount}`,
        );
      }
    },
    damage(min, max, user, move) {
      const damage = Math.max(Math.floor(Math.random() * max) + 1, min);
      if (user === 'player' && this.$props.playerHealth >= 0) {
        this.$emit('onUpdateHealth', 'opponent', damage);
        this.gameLog.splice(0, 0,
          `User ${move} opponent resulting in damage of ${damage}`,
        );
        this.checkWinner('opponent', damage);
      } else if (this.$props.opponentHealth >= 0) {
        this.$emit('onUpdateHealth', 'player', damage);
        this.gameLog.splice(0, 0,
          `Opponent retaliated causing you damage of ${damage}`,
        );
        this.checkWinner('player', damage);
      }
    },
    styling(idx) {
      if (idx % 2 === 0) {
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
  font-weight: normal;
  text-transform: none;
  color: #fff;
}

ul > li {
  padding: 5px;
    width: auto;
}

    .row.log {
        height: 60vh;
        overflow-y: scroll;
    }

</style>
