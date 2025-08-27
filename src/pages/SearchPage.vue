<template>
  <div class="search">
    <h1>Search Recipes</h1>
    <form @submit.prevent="searchRecipes">
      <div class="form-group">
        <input v-model="searchQuery" type="text" placeholder="Search recipes or ingredients..." class="form-control" />
      </div>
      <div class="form-group">
        <input v-model.number="numResults" type="number" step="5" min="5" max="25" placeholder="Number of results" class="form-control" />
      </div>  
      <button type="submit" class="btn btn-success mt-3">Search</button>
    </form>
  </div>
  <div class="search-results" v-show="showResults">
    <span>
      <RecipePreviewList title="Search results" :recipes="results" />
    </span>
  </div>
</template>

<script>
import axios from 'axios';
import store from '@/store';
import RecipePreviewList from '@/components/RecipePreviewList.vue';

const server_domain = store.server_domain;

export default {
  name: "SearchPage",

  data() {
    return {
      searchQuery: '',
      numResults: 5,
      results: [],
      showResults: false,
    }
  },

  components: {
    RecipePreviewList
  },

  methods: {
    async searchRecipes() {
      this.showResults = false;
      const recipes = await axios({
        method: "GET",
        url: server_domain + '/recipes/search',
        validateStatus: () => true,
        data: {
          query: this.searchQuery,
          number: this.numResults,
        }
      });

      if (recipes.status === 500) {
        alert("Out of Spoonacular points")
        return
      }
      this.results = recipes.data; 
      this.showResults = true;
    }
  },
}
</script>


<style scoped>
.search {
  max-width: 400px;
  margin: auto;
}

.search h1 {
  text-align: center;
}

.search button {
  position: relative;
  left: 40%;
}

.search-results {
  margin-top: 20px;
  padding: 20px;
  border-top: solid 2px rgba(0, 0, 0, 0.31);
  background: linear-gradient(180deg, rgba(139,167,32,0) 0%, rgba(97, 149, 159, 0.532) 120%);
}
</style>