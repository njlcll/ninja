<template>
  <div class="index container">
    <h4 class="p-4">Home</h4>
    <div class="row ">
      <div class="col-sm-6  col-lg-4  "
        v-for="smoothie in smoothies"
        :key="smoothie.id"
      >
        <div class=" text-center   m-3">
          <h4 class=" mytitle">{{ smoothie.title }}</h4>

          <ul class="ingredients list-group mycard ">
            <li
              class="list-group-item "
              v-for="(ingredient, index) in smoothie.ingredients"
              :key="index"
            >
              <span class="badge p-2 badge-warning">{{ ingredient }}</span>
            
            </li>
          </ul>

          <div class="bottom align-baseline">
          <router-link
            :to="{
              name: 'EditSmoothie',
              params: { smoothie_slug: smoothie.slug },
            }"
            ><i class="material-icons mt-2">edit</i></router-link
          >

          <a href="#" @click="del(smoothie.id)"
            ><i class="material-icons">delete</i></a
          >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import db from "../firebase/init";

export default {
  name: "Index",
  components: {},
  data() {
    return {
      smoothies: [],
    };
  },
  methods: {
    del(id) {
      db.collection("smoothies")
        .doc(id)
        .delete()
        .then(() => {
          this.smoothies = this.smoothies.filter((smoothie) => {
            return smoothie.id != id;
          });
        });
    },
  },
  created() {
    // fetch data from firestore
    db.collection("smoothies")
      .get()
      .then((snapshot) => {
        snapshot.forEach((doc) => {
          let smoothie = doc.data();
          smoothie.id = doc.id;
          this.smoothies.push(smoothie);
        });
      });
  },
};
</script>

<style >

.mytitle{
  color:cornflowerblue;
}


</style>