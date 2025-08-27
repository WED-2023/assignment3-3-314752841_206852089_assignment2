<template>
  <div id="app">
    <div id="nav">
      <div id="logo" class="nav-items"></div>
      
      <welcome-message style="text-transform: capitalize;"/>

      <div class="nav-items">
        <router-link :to="{ name: 'main' }">Home</router-link>
      </div>
      <div class="nav-items">
        <router-link :to="{ name: 'search' }">Search</router-link>
      </div>
      <div class="nav-items">
        <div class="dropdown">
          <a class="nav-items">Personal</a>
          <div class="dropdown-content" v-if="store.username">
            <router-link :to="{ name: 'favorites'}">Favorites &hearts;</router-link>
            <router-link :to="{ name: 'private'}">Private</router-link>
            <router-link :to="{ name: 'family'}">La Familia</router-link>
          </div>
          <div class="dropdown-content" v-else>
            <router-link :to="{ name: 'login'}">Favorites &hearts;</router-link>
            <router-link :to="{ name: 'login'}">Private</router-link>
            <router-link :to="{ name: 'login'}">La Familia</router-link>
          </div>
        </div>
      </div>
      <div class="nav-items">
        <a @click="openModal">Create Recipe</a>
      </div>
      <div class="nav-items">
        <router-link :to="{ name: 'about' }">About</router-link>
      </div>
      <div v-if="!store.username" class="nav-items">
        <router-link :to="{ name: 'register' }">Register</router-link>
        <router-link :to="{ name: 'login' }">Login</router-link>
      </div>
      <div v-else class="nav-items">
        <a @click="logout">Logout</a>
      </div>
    </div>
    <div id="myModal" class="modal">
      <div class="modal-content">
        <span @click="closeModal" class="close">&times;</span>
        <h1>Create a new recipe</h1>
        <p v-if="!store.username">To create a new recipe, please <router-link :to="{ name: 'login' }" @click="closeModal">login</router-link> or <router-link :to="{ name: 'register' }" @click="closeModal">register</router-link> if you don't have an account yet.</p>
        <div v-else>
          <form @submit.prevent="createRecipe">
            <div class="form-group">
              <input type="text" placeholder="Recipe name/title" class="form-control" />
            </div>
            <div class="form-group">
              <input type="number" placeholder="Preparation time (in minutes)" class="form-control" />
            </div>
            <div class="form-group">
              <input type="text" placeholder="Description" class="form-control"/>
            </div>
            <div class="form-control">
              <input type="checkbox" id="vegan"/>
              <label for="vegan">Vegan?</label>
              <br/>
              <input type="checkbox" id="vegetarian"/>
              <label for="vegetarian">Vegetarian?</label>
              <br/>
              <input type="checkbox" id="gluten"/>
              <label for="gluten">Gluten free?</label>
            </div>
            <button type="submit" class="btn btn-success mt-3" style="margin-right: 8px;">Create Recipe</button>
            <button type="reset" class="btn btn-success mt-3">Clear</button>
          </form>
        </div>
      </div>
    </div>
    <router-view />
  </div>
</template>

<script>
import { getCurrentInstance } from 'vue';
import WelcomeMessage from './components/WelcomeMessage.vue';
// import axios from 'axios';
// import store from '../store';

// const server_domain = store.server_domain;

export default {
  name: "App",
  setup() {
    const internalInstance = getCurrentInstance();
    const store = internalInstance.appContext.config.globalProperties.store;
    const toast = internalInstance.appContext.config.globalProperties.toast;
    const router = internalInstance.appContext.config.globalProperties.$router;

    const logout = () => {
      store.logout();
      toast("Logout", "User logged out successfully", "success");
      router.push("/").catch(() => {});
    };

    const openModal = () => {
      var modal = window.document.getElementById("myModal");
      modal.style.display = "block";
    }

    const closeModal = () => {
      var modal = window.document.getElementById("myModal");
      modal.style.display = "none";
    }

    const createRecipe = () => {
      
      // ToDo implement create recipe - NEED TO UPDATE BACKEND AS WELL
    }

    window.onclick = (event) => {
      if (event.target === window.document.getElementById("myModal")) {
        closeModal();
      }
    }

    return { store, logout, openModal, closeModal, createRecipe };
  },

  components: {
    WelcomeMessage
  }
}
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;

  background-color: seashell;
}

#nav {
  padding: 10px;
  display: flex;

  background-color: hwb(180 90% 10%);
  border-radius: 4px;

  justify-content: flex-start;
  align-items: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
  z-index: 10;

  text-align: center;
  text-transform: uppercase;
  font-weight: 600;
  box-shadow: 0 2px 8px lightblue;
  text-decoration: wavy;
}

#logo {
  background-image: url('@/assets/logo.jpg');
  background-size: cover;
  
  height: 45px;
  width: 45px;

  margin-right: 50px;
  border-radius: 40%;

  box-shadow: #2c3e50 0 0 10px 2px;
  flex: none;
}

#logo:hover {
  box-shadow: #0e2946 0 0 10px 2px;
}

.nav-items a {
  text-decoration: none;
  padding: 0.5rem 1rem;
  color: inherit;
  font-weight: 600;

  cursor: pointer;
}

#nav a:hover {
  background-color: rgba(0, 0, 0, 0.05);
  border-radius: 4px;

  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.dropbtn {
  background-color: inherit;
  color: inherit;
  font-weight: inherit;
  cursor: pointer;
  border: none;
  text-transform: uppercase;
}

.dropbtn:hover {
  background-color: rgba(0, 0, 0, 0.05);
  border-radius: 4px;

  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f1f1f1;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
  z-index: 1;

  border-radius: 10%;
}

.dropdown-content a {
  padding: 12px 16px;
  text-decoration: none;
  text-transform: capitalize;
  display: block;
  transition: background-color 0.3s ease;
}

.dropdown:hover .dropdown-content {
  display: block;
}

    
.dropdown-content a:hover {
  background-color: #ddd;
  /* background-color: red; */

  border-radius: 10%;
}

#nav a.router-link-exact-active {
  color: rgb(94, 94, 152);
}


// The Modal (background)
.modal {
  display: none; /* Hidden by default */
  position: absolute; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgba(0,0,0,0.8); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%; /* Could be more or less, depending on screen size */
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  text-align: center;
  font-size: 28px;
  font-weight: bold;
  width: 28px;
  height: 28px;
  margin-bottom: 10px;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
