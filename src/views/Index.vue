<template>
  <div class="index">
    <h4>home view</h4>
    <ul>
      <li v-for="smoothie in smoothies" :key="smoothie.id">{{smoothie.title}} <a href='#' @click='del(smoothie.id)'>x </a></li>
    </ul>
          
  </div>
</template>

<script>
// @ is an alias to /src
import { db } from '../firebase/init'

export default {
  name: 'Index',
  components: {

  },
  data(){
    return {
      smoothies : []
    }
  },
  methods :{
    del(id){
      this.smoothies.splice(id-1,1);
    }
  },
  created(){
    // fetch data from firestore
    db.collection('smoothies').get()
    .then(snapshot => {
      snapshot.forEach(doc => {
        console.log(doc.data())
        let smoothie = doc.data()
        smoothie.id = doc.id
        this.smoothies.push(smoothie)
      })
    })
  }
}
</script>
