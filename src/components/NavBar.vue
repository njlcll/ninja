<template>
  <div id="navbar">
<h2>Ninja Smoothies</h2>
      <router-link to="/about">About</router-link> |
      <router-link :to="{ name: 'Index' }">Home</router-link> |
    <router-link :to="{ name: 'AddSmoothie' }"><i class="material-icons float-right" style="font-size:48px;color:red">add_circle</i></router-link> |


       

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
#navbar{
    background-color: cadetblue;
    height: 100px;
}
</style>