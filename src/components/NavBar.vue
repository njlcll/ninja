<template>
  <div id="navbar">
    <h2 class='text-center p-3 ' style="color : white">Smoothie Recipes</h2>
    <router-link :to="{ name: 'Index' }"
      ><i class="material-icons m-2" style="font-size: 36px; color: white">home</i></router-link
    >
     <router-link :to="{ name: 'AddSmoothie' }"
      ><i class="material-icons m-2" style="font-size: 36px; color: white"
        >add_circle</i
      ></router-link
    >
    
    <router-link to="/about"
      ><i class="material-icons m-2" style="font-size: 36px; color: white"
        >info</i
      ></router-link
    >

   
  </div>
</template>

<script>
//test branch
export default {
  name: "NavBar",
  data() {
    return {};
  },
};
</script>

<style>
#navbar {
  background-color: cadetblue;
  
}
</style>
