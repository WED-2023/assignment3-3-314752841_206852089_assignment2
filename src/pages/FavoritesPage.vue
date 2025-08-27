<template>
  <div class="favorites-page">
    <h1>Favorites &hearts;</h1>
  </div>
</template>

<script>

export default {
  name: 'FavoritesPage',
  components: {
  },
  data() {
    return {
      recipes: [],
    };
  },
};
</script>

<style scoped>
.favorites-page {
  max-width: 400px;
  margin: auto;
}

.favorites-page h1 {
  text-align: center;
}
</style>