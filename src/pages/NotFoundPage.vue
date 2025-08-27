<template>
  <div class="not-found-page">
    <h1>Four Oh Four you didn't</h1>  
    <p>
      The page you are looking for doesn't exist it's all a <span>fugazi</span> it's a woozy it's a wazzy. 
    </p>
    <router-link to="/" exact class="btn btn-success mt-3">Home</router-link>
  </div>
</template>

<style>
.not-found-page {
  padding: 1rem;
}

.not-found-page span {
  font-style: italic;
  font-weight: 600;
  font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif ;
}
</style>