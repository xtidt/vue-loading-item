<template>
  <div id="app">
    <img src="./assets/logo.png"><br>
    <loading-item></loading-item>

    <div v-for="user in users" class="items" v-if="loading">
      <user-item :name="user.name" :email="user.email"></user-item>
    </div>

    <div class="lt20 text-center" v-for="load in loades" v-if="!loading">
      <loading-item></loading-item>
    </div>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld';
import UserItem from './components/UserItem';
import LoadingItem from './components/LoadingItem';

export default {
  name: 'App',
  components: {
    HelloWorld,
    UserItem,
    LoadingItem
  },
  data() {
    return {
      users: [],
      loading: false,
      loades: 10
    }
  },
  methods: {
    getUserDetails: function() {
      fetch(`https://jsonplaceholder.typicode.com/users`)
        .then(result => result.json())
        .then(result => {
          this.users = result,
          this.loading = true
        });
    }
  },
  created: function() {
    setTimeout(() => {
      this.getUserDetails()
    }, 3000);
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.lt20 {
  padding: 20px 0;
}

.text-center {
  text-align: center;
}
</style>
