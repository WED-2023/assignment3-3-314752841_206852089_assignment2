<template>
  <b-container>
    <h3 style="color:cadetblue; text-decoration:underline;">
      {{ title }}:
      <slot></slot>
    </h3>
    <div id="recipe-preview-list">
      <RecipePreview v-for="recipe in recipes" :key="recipe.id" :recipe="recipe" />
    </div>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    title: {
      type: String,
      required: true
    },
    recipes: {
      type: Array,
    },
  },
};
</script>

<style lang="scss" scoped>  
.container {
  min-height: 400px;
}

#recipe-preview-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-evenly; 
}
</style>
