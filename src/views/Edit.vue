<template>

  <div   class="container">

    <div vi-if="smoothie" class="title"><h4 class='p-4'>Edit {{smoothie.title}}</h4></div>
    

        <form @submit.prevent="editSmoothie">
      <div class="form-group">
        <label for="title">Edit  title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          placeholder="title"
          v-model="smoothie.title"
        />
      </div>

      <div class="form-group">
        <ul class="ingredients list-group">
          <li
            class="list-group-item"
            v-for="(ingredient, index) in smoothie.ingredients"
            :key="index"
          >
            <span class="badge p-2 badge-warning">{{ ingredient }}</span>
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
    name : 'EditSmoothie',
    data (){
      return {
        smoothie : "",
        another : null,
        feedback :null
      }
    },
    methods :{
      editSmoothie() {
      if(this.smoothie.title){
        this.feedback = null;
        this.smoothie.slug = slugify(this.smoothie.title, {
          replacement : "-",
          remove: /[$*_+~.()'"!\-:@]/g,
          lower : true
        })
        db.collection("smoothies").doc(this.smoothie.id).update({
          title :this.smoothie.title,
          ingredients :this.smoothie.ingredients,
          slug :this.smoothie.slug
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
        this.smoothie.ingredients.push(this.another);
        this.another = null;
        this.feedback = null;
      } else {
        this.feedback = "Please enter the ingredient";
      }
    },
    del(id) {
      this.smoothie.ingredients.splice(id - 1, 1);
    },
    },
    created (){
      let ref = db.collection('smoothies').where('slug', '==',this.$route.params.smoothie_slug)
      ref.get().then(snapshot =>{
        snapshot.forEach(doc => {
                this.smoothie = doc.data()
                this.smoothie.id = doc.id
                console.log(this.smoothie)
            })
        
      }
        
      )
    }
}
</script>

<style>

</style>