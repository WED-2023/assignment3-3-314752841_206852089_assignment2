<template>
  <div class="about-page">
    <h1>About Us</h1>
    <p class="section">Michrot Recipes is a social media platform built for food lovers, home cooks, and culinary explorers. Our mission is to make discovering, sharing, and preserving recipes easier and more personal than ever.
        Whether you're searching for your next favorite dish or sharing a cherished family recipe, Michrot Recipes brings people together through food. Use our Search page—powered by the Spoonacular API—to find recipes from across the web, or visit your Personal Dashboard to save favorites, upload private creations, and keep treasured family recipes all in one place.
        Join a community where food meets connection, and every recipe tells a story.</p>
    <h1>Walkthrough</h1>
    <p class="section">
      New users: <br />
      To use all features this site can provide - <br />
          1. Register a new user <br />
          2. Log in <br />
          3. Access Personal dashboard and start saving / sharing recipes!
    </p>
    <h1>Other Links</h1>
    <p class="section">
      <a href="https://wed-2023.github.io/314752841/">Assignment 1 - Michael</a> <br />
      <a href="https://wed-2023.github.io/assignment2-314752841_206852089_assignment2/">Assignment 2 - Space Invaders</a>
    </p>
    <h1>Credits</h1>
    <p class="section end">
      This is a third year fullstack project. <br />
      Course: Web Developement Environments :: Department of Software and Information Systems Engineering :: Ben-Gurion University of the Negev <br />
      Collaborators: Michael Livshits, Rotem Atar
    </p>
  </div>
</template>

<script>
export default {
  name: 'AboutPage',
};
</script>

<style scoped>
.about-page {
  padding: 2rem;
}

.about-page h1 {
  color: rgba(20, 50, 52, 0.669);
}

.section {
  padding: 1rem;

  border-bottom: groove 1rem;
  border-width: 2px;
  border-color: rgba(0, 255, 255, 0.308);

  font-weight: 600;
}

.end {
  border: none;
  padding-bottom: 0;
}

a {
  color: #3498db;
  text-decoration: none;
}
</style>