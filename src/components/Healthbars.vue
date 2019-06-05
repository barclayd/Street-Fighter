<template>
  <div>
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">You</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            style="background-color: green; margin: 0; color: white;"
          ></div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">Opponent</h1>
        <div class="healthbar">
          <div
            class="healthbar text-center"
            style="background-color: green; margin: 0; color: white;"
          ></div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Healthbar',
};
</script>

<style scoped></style>
