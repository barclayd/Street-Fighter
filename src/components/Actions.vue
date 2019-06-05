<template>
  <div>
    <section class="row controls">
      <div class="small-12 columns">
        <button id="start-game">START NEW GAME</button>
      </div>
    </section>
    <section class="row controls">
      <div class="small-12 columns">
        <button id="attack">Punch</button>
        <button id="special-attack">Kick</button>
        <button id="heal">HEAL</button>
        <button id="give-up">GIVE UP</button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Actions',
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
