<template>
  <div>
    <h1>{{ name }}</h1>
    <input type="text" v-model="name">
    <button @click="onClick">click</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: 'About me in awesome'
    };
  },
  methods: {
    onClick() {
      alert(this.name);
    }
  }
}
</script>

<style scoped>

</style>
