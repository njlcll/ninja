<template>
  <div id="navbar">
      Nav bar
      <router-link to="/about">About</router-link> |
      <router-link :to="{ name: 'Index' }">Home</router-link> |
       <i class="material-icons">add</i>
      </div>
</template>

<script>
export default {
    name : "NavBar",
    data(){
        return {

        }
    }
}
</script>

<style>

</style>