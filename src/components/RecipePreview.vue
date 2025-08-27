<template>
<RouterLink :to="{ name: 'recipes', params: { recipeId: recipe.id } }">
  <div class="recipe-preview">
    <div id="thumbnail" :style="{ backgroundImage: `url(${image})`}">
      <div id="info" class="blur-box">
        vegan: <span v-html="vegan ? '&#10003;' : '&#10007;'"></span><br/>
        vegetarian: <span v-html="vegertarian ? '&#10003;' : '&#10007;'"></span><br/>
        glutenFree: <span v-html="glutenFree ? '&#10003;' : '&#10007;'"></span><br/> 
      </div>
      <div id="time-likes" class="blur-box">
        <span>&#128339; {{ readyInMinutes }}</span>
        |
        <span>&#128525; {{ aggregateLikes }}</span>
      </div>
    </div>

    <div class="description">
      {{ title }} <br />
      {{ description }}
    </div>
  </div>
</RouterLink>
</template>

<script>
export default {
  name: "RecipePreview",
  props: {
    recipe: {
      type: Object,
      required: true
    }
  },

  data() {
    return {
      id: this.recipe.id,
      image: this.recipe.image || 'No picture',
      title: this.recipe.title,
      description: this.recipe.description || 'No description available.',
      readyInMinutes: this.recipe.readyInMinutes,
      aggregateLikes: this.recipe.aggregateLikes,
      vegertarian: this.recipe.vegetarian,
      vegen: this.recipe.vegan,
      glutenFree: this.recipe.glutenFree,
    }
  }
}
</script>

<style scoped>
a:link {
  text-decoration: none;
}

.recipe-preview {
  margin: 15px;
  border: 1px solid rgba(0, 0, 0, 0.31);
  overflow: hidden;
  width:350px;
  height:200px;
  position: relative;
  background-color: rgba(0, 255, 255, 0.154);
  border-radius: 3%;
  border-width: 2px;
  box-shadow: 10px 10px 20px 2px #ccc;
}

.recipe-preview:hover {
  box-shadow: 10px 10px 20px 2px #aaa;
  cursor: pointer;
}

.recipe-preview::after {
  content: '';
  position: absolute;
  left: 0px;
  right: 0px;
  height: 50%;
  bottom: 0px;
  background: linear-gradient(180deg, rgba(139,167,32,0) 0%, rgba(255,255,255,1) 110%);
  pointer-events: none;
}

#thumbnail {
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  height: 60%;
  width: 100%;
  border-radius: 3% 3% 0% 0%;
  border-bottom: solid 3px grey;
}

.description {
  padding-left: 5px;
  padding-right: 5px;
  height: 38.5%;
  overflow: hidden;  
  border-radius: 0 0 3% 3%;
  font-weight: 600;
  font-style: italic;
  font-family: 'Times New Roman', Times, serif;
}

.blur-box {
  box-shadow: 0 0 20px 8px rgba(0,0,0,0.15);
  box-shadow: inset 0 0 10px rgba(0,0,0,0.15);
}

#info {
  padding-left: 3px;  
  overflow: hidden;
  width: 20%;
  height: 35%;

  backdrop-filter: blur(10px);
  background: rgba(0, 0, 0, 0.28);
  color: white;

  border-radius: 0 0% 15% 0%;

  font-size: xx-small;
  font-weight: 600;
}

#time-likes {
  padding-left: 2px;  
  overflow: hidden;
  font-weight: 600;
  font-size: xx-small;
  position: sticky;
  width: 21%;
  top: 60%;

  backdrop-filter: blur(3px);
  background: rgba(0, 0, 0, 0.28);
  color: white;
  border-radius: 0 15% 0% 0 0%;
}
</style>
