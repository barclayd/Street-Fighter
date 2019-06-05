<template>
  <div id="app">
    <Healthbars />
    <Actions />
    <ActionLog />
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
};
</script>

<style></style>
