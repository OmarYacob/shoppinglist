<template>
  <q-page class =bg-grey-2 column>
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newIngredients"
        @keyup.enter="addIngredients"
        class="col"
        square 
        filled
        bg-color="white"
        placeholder="Add ingredients" 
        dense>
        <template v-slot:append>
          <q-btn 
            @click="addIngredients"
            round 
            dense 
            flat 
            icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white"
    separator
    bordered>
      <q-item 
      v-for="(ingredients, index) in ingredients"
      :key="ingredients.title"
      @click="ingredients.done = !ingredients.done"
      :class="{ 'done bg-green-2' : ingredients.done}"
      clickable
      v-ripple>
        <q-item-section avatar>
          <q-checkbox 
          v-model="ingredients.done"
          class="no-pointer-events" 
          color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ingredients.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="ingredients.done"
          side>
          <q-btn 
          @click.stop="deleteIngredients(index)"
          flat 
          round
          dense
          color="primary" 
          icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div 
      v-if="!ingredients.length"
      class="no-ingredients absolute-center">
      <q-icon 
      name="check"
      size="100px"
      color="primary"/>
      <div class="text-h5 text-primary center">
        No ingredients in
        <br>
         your shoppinglist
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data(){
    return{
      newIngredients: '',
      ingredients:[
       // {
       //     title: 'Get potatoes',
       //     done: false
     //   },
       // {
      //      title: 'Get onions',
       //     done: false
      //  },
     //   {
      //      title: 'Get nudles',
       //     done: false
     //   }
      ]
    }
  },
  methods: {
    deleteIngredients(index){

      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.ingredients.splice(index, 1)
        this.$q.notify('Ingredients deleted')
      })
    },
    addIngredients() {
      this.ingredients.push({
        title: this.newIngredients,
        done: false
      })
      this.newIngredients = ''
    }  
  }
}
</script>

<style lang="scss">
.done{
  .q-item__label{
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-ingredients{
  opacity: 0.5;
}
</style>
