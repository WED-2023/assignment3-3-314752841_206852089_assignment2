<template>
  <div class="welcome-message">
    <div v-if="!store.username" >
      Hello, welcome to MichRot Recipes.
    </div>
    <div v-else>
      Welcome back, {{ store.username }}!
    </div>
  </div>
</template>

<script>
export default {
  name: 'WelcomeMessage',
};
</script>

<style scoped>
.welcome-message {
  display: flex;

  flex: 50px;
}
</style>