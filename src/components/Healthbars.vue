<template>
  <div>
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">You</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            :style="displayHealth(playerHealth)"
          >
            {{ playerHealth }}
          </div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">Opponent</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            :style="displayHealth(opponentHealth)"
          >
            {{ opponentHealth }}
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Healthbars',
  props: ['player-health', 'opponent-health'],
  methods: {
    displayHealth(user) {
      let healthBarColour = 'green';
      if (user < 20 ) {
        healthBarColour = '#bb0a1e';
      }
      return [
        { backgroundColor: healthBarColour, margin: 0, color: 'white' },
        { width: user + '%' },
      ];
    },
  },
};
</script>

<style scoped></style>
