<template>
<div class="recipe-container" v-if="recipe">
  <div class="recipe-header">
    <h1 class="recipe-title"> {{ recipe.title }}</h1>
    <p class="recipe-meta">{{ recipe.readyInMinutes }} mins • {{ recipe.aggregateLikes }} likes</p>
  </div>

  <img :src="recipe.image" class="recipe-image">

  <div class="recipe-info">
    <div class="recipe-section" id="ingredients">
      <h2 class="section-title">Ingredients</h2>
      <ul>
        <li>200g spaghetti</li>
        <li>2 eggs</li>
        <li>100g pancetta</li>
        <li>Salt & pepper</li>
      </ul>
    </div>

    <div class="recipe-section collapsible" id="instructions">
      <h2 class="section-title">Instructions</h2>
      <ol>
        <li>Boil pasta.</li>
        <li>Cook pancetta until crispy.</li>
        <li>Mix eggs and cheese.</li>
        <li>Combine everything and serve.</li>
      </ol>
    </div>

    <div class="recipe-section collapsible" id="notes">
      <h2 class="section-title">Notes</h2>
      <p>Make sure the pasta is hot when mixing with eggs to cook them slightly.</p>
    </div>
  </div>

  <div class="recipe-actions">
    <button class="favorite-btn" @click="saveToFavorites">❤️ Favorite</button>
    <button class="share-btn">🔗 Share</button>
  </div>
</div>
</template>
  
<script>
  export default {
    data() {
      return {
        recipe: null
      };
    },
    async created() {
      try {
        let response;
        response = this.$route.params.response;

        try {
          response = await this.axios.get(
            this.$root.store.server_domain + "/recipes/" + this.$route.params.recipeId,
          );

          console.log("response.status", response.status);
          if (response.status !== 200) {
            this.$router.replace("/NotFound");
            
          }
        } catch (error) {
          console.log("error.response.status", error.response.status);
          this.$router.replace("/NotFound");
          return;
        }

        let {
          analyzedInstructions,
          instructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title
        } = response.data;

        let _instructions = analyzedInstructions
          .map((fstep) => {
            fstep.steps[0].step = fstep.name + fstep.steps[0].step;
            return fstep.steps;
          })
          .reduce((a, b) => [...a, ...b], []);

        let _recipe = {
          instructions,
          _instructions,
          analyzedInstructions,
          extendedIngredients,
          aggregateLikes,
          readyInMinutes,
          image,
          title
        };

        this.recipe = _recipe;
      } catch (error) {
        console.log(error);
      }
    },
    methods: {
      // async saveToFavorites() {   // Fix this
      //   const res = await this.axios({
      //     method: "POST",
      //     url: this.$root.store.server_domain + "/user/favorites",
      //     validateStatus: () => true,
      //     data: {
      //       recipeId: this.$route.params.recipeId,
      //       user_id: this.$root.store.username,
      //     }
      //   })

      //   if (res.status === 200) {
      //     alert("Recipe saved to favorites!");
      //   } else {
      //     alert("Failed to save recipe to favorites.\n" + res.status + ": " + res.data.message);
      //   }
      // }
    }
  };
</script>

<style scoped>
/* General Layout */
.recipe-container {
  max-width: 700px;
  border-radius: 16px;
  margin: 2rem;
  padding: 2rem;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.2);
  transition: all 0.3s ease;
}

.recipe-header {
  text-align: center;
  margin-bottom: 1.5rem;
}

.recipe-title {
  font-size: 2rem;
  margin-bottom: 0.3rem;
}

.recipe-meta {
  color: #888;
  font-size: 0.9rem;
}

/* Image Placeholder */
.recipe-image {
  width: 100%;
  height: 300px;
  background: linear-gradient(135deg, #eee, #ccc);
  border-radius: 12px;
  margin-bottom: 1.5rem;
  background-size: cover;
  background-position: center;
  animation: pulse 3s infinite ease-in-out;
}

/* Pulsing effect for image */
@keyframes pulse {
  0% { opacity: 1; }
  50% { opacity: 0.9; }
  100% { opacity: 1; }
}

.recipe-info {
  line-height: 1.6;
}

.recipe-section {
  margin-bottom: 1.5rem;
  border-left: 4px solid #ddd;
  padding-left: 1rem;
  transition: all 0.3s ease;
}

.section-title {
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
  cursor: pointer;
  position: relative;
}

.section-title::after {
  content: '▼';
  font-size: 0.8rem;
  position: absolute;
  right: 0;
  transition: transform 0.3s;
}

.collapsible.collapsed .section-title::after {
  transform: rotate(-90deg);
}

.collapsible.collapsed ol,
.collapsible.collapsed ul,
.collapsible.collapsed p {
  display: none;
}

/* Buttons */
.recipe-actions {
  display: flex;
  justify-content: space-around;
  margin-top: 2rem;
}

button {
  padding: 0.6rem 1.2rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

</style>
  