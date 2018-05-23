<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <h2>{{ name }}</h2>
      <p>{{ email }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'user-item',
  props: ['email', 'name'],
  data() {
    return {
        msg: 'Skeleton Screen'
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
