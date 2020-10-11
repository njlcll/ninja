<template>
  <div class="container">
    <h2>Add smoothie</h2>
    <form @submit.prevent="addSmoothie">
      <div class="form-group">
        <label for="title">Add a title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          placeholder="title"
          v-model="title"
        />
      </div>

      <div class="form-group">
        <ul class="ingredients list-group">
          <li
            class="list-group-item"
            v-for="(ingredient, index) in ingredients"
            :key="index"
          >
            <span class="badge badge-primary">{{ ingredient }}</span>
            <a href="#" class="float-right" @click="del(index)"><i class="material-icons">delete</i></a>
          </li>
        </ul>

        <label for="ingredient">Add an ingredient</label>
        <div class="input-group-prepend">
          <input
            @keydown.tab.prevent.enter="addIngredient"
            v-model="another"
            type="text"
            class="form-control d-inline-block"
            id="ingredient"
            placeholder="ingredient"
          />
          <button type='button' @click="addIngredient" class="btn btn-secondary btn-sm ml-2">
            Add
          </button>
        </div>
      </div>
      <p v-if="feedback" class="alert alert-warning">{{ feedback }}</p>
      <button type="submit" class="btn btn-primary mb-2">Save</button>
    </form>
  </div>
</template>

<script>
import db from '../firebase/init'
import slugify from 'slugify'
export default {
  name: "AddSmoothie",
  data() {
    return {
      title: null,
      another: null,
      feedback: null,
      ingredients: [],
      slug :null
    };
  },
  methods: {
    addSmoothie() {
      if(this.title){
        this.feedback = null;
        this.slug = slugify(this.title, {
          replacement : "-",
          remove: /[$*_+~.()'"!\-:@]/g,
          lower : true
        })
        db.collection("smoothies").add({
          title :this.title,
          ingredients :this.ingredients,
          slug :this.slug
        }).then(()=>{
            this.$router.push({name: 'Index'})
          }
        ).catch (e => {
          console.log(e)
        })
      }
      else{
        this.feedback ="You must enter a smoothie title"
      }
    },
    addIngredient() {
      if (this.another) {
        this.ingredients.push(this.another);
        this.another = null;
        this.feedback = null;
      } else {
        this.feedback = "Please enter the ingredient";
      }
    },
    del(id) {
      this.ingredients.splice(id - 1, 1);
    },
  },
};
</script>

<style>
</style>