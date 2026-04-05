<template>
  <div class="profile">
    <h1>Профіль користувача</h1>

    <nav class="sub-nav">
      <router-link to="/profile/info">Інформація</router-link>
      <router-link to="/profile/settings">Налаштування</router-link>
    </nav>

    <router-view />
  </div>
</template>

<style>
.profile {
  padding: 20px;
}

.sub-nav {
  margin: 15px 0;
}

.sub-nav a {
  margin-right: 15px;
  text-decoration: none;
  color: #42b983;
}

.sub-nav a.router-link-active {
  font-weight: bold;
  border-bottom: 2px solid #42b983;
}
</style>