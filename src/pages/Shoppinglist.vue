<template>
  <q-page class="bg-grey-2" column>
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newIngredient"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Bezeichnung"
        dense
      ></q-input>
      <q-input
        v-model="newUnit"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Einheit"
        dense
      ></q-input>
      <q-input
        v-model="newAmount"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Menge"
        dense
      ></q-input>

      <q-btn @click="addIngredients" round dense flat icon="add"></q-btn>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(ingredient, index) in ingredients"
        :key="ingredient.title"
        @click="ingredient.done = !ingredient.done"
        :class="{ 'done bg-green-2' : ingredient.done}"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="ingredient.done" class="no-pointer-events" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ingredient.title }}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ingredient.unit }}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ingredient.amount }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="ingredient.done" side>
          <q-btn
            @click.stop="deleteIngredients(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!ingredients.length" class="no-ingredients absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary center">
        No ingredients in
        <br />your shoppinglist
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data: () => {
    newIngredient;
    newUnit;
    newAmount;
  },
  computed: {
    ingredients: {
      get() {
        return this.$store.state.shoppinglist.ingredients;
      },
      set() {
        this.$store.commit("shoppinglist/updateIngredients", val);
      }
    }
  },
  methods: {
    deleteIngredients(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Really delete?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.ingredients.splice(index, 1);
          this.$q.notify("Ingredients deleted");
        });
    },
    addIngredients() {
      this.ingredients.push({
        title: this.newIngredient,
        unit: this.newUnit,
        amount: this.newAmount,
        done: false
      });
      this.newIngredient = "";
      this.newUnit = "";
      this.newAmount = "";
    }
  }
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-ingredients {
  opacity: 0.5;
}
</style>
