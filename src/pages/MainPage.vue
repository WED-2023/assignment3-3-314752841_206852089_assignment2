<template>
  <div class="main-page">
    <h1 class="title">Main Page</h1>
    <div class="content">
      <RecipePreviewList title="Random Recipes" class="RandomRecipes" :recipes="randomRecipes" />

      <div v-if="!store.username" class="text-center mt-4">
        <router-link :to="{ name: 'login' }">
          <button class="btn btn-success mt-3">You need to Login to view this</button>
        </router-link>
      </div>

      <RecipePreviewList
        title="Last Viewed Recipes"
        :class="{
          RandomRecipes: true,
          blur: !store.username,
        }"
        disabled
        :recipes="randomRecipes"
      />
    </div>
  </div>
</template>

<script>
import { getCurrentInstance } from 'vue';
import RecipePreviewList from "../components/RecipePreviewList.vue";
import axios from 'axios';
import store from '../store';

const server_domain = store.server_domain;

export default {
  components: {
    RecipePreviewList,
  },
  setup() {
    const internalInstance = getCurrentInstance();
    const store = internalInstance.appContext.config.globalProperties.store;

    return { store };
  },

  data() {
    return {
      randomRecipes: [],
    }
  },

  mounted() {
    this.getRandoms();
    // this.template();
  },

  methods: {
    async getRandoms() {
      const recipes = await axios({
        method: "GET",
        url: server_domain + '/recipes/random',
        validateStatus: () => true,
      });

      if (recipes.status === 500) {
        console.log("Random failed - server error 500")
        return
      }
      this.randomRecipes = recipes.data;
    },

    template() {
      let recipes = [
        {
          id: 1,
          title: 'Pasta',
          description: 'This is a pasta dish, made by the finest of italians, best served with coffee and cigaretts ',
          readyInMinutes: 10,
          aggregateLikes: 200,
          vegertarian: true,
          vegen: false,
          glutenFree: false,
        },
        {
          id: 2,
          title: 'Steak',
          description: 'This is a steak dish, its very tasty and nutrtional, it has lots of meat and proteins for a healthy body and lifestyle!',
          readyInMinutes: 15,
          aggregateLikes: 142,
          vegertarian: false,
          vegan: false,
          glutenFree: true,
        },
        {
          id: 3,
          title: 'Beans',
          description: 'Beans, beans its english it has no taste but its good for your health',
          readyInMinutes: 4,
          aggregateLikes: 92,
          vegertarian: true,
          vegan: true,
          glutenFree: false,
        }
      ];
      
      this.randomRecipes = recipes;
    }
  }
};
</script>

<style lang="scss" scoped>
.main-page {
  padding: 2rem;
}
.content {
  column-count: 2;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(5px);
  pointer-events: none;
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
