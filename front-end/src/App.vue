<script setup>
// IMPORT LIBS
import { onMounted, ref } from 'vue';
import axios from 'axios';

// IMPORT COMPONENTS
import PizzaIndex from './components/PizzaIndex.vue';
import PizzaShow from './components/PizzaShow.vue';
import PizzaForm from './components/PizzaForm.vue';

// DATA
const pizzas = ref(null);
const pizzaActive = ref(null);
const creatingPizza = ref(false);

// FUNCTIONS
const getPizzas = async () => {
  const data = await axios.get("http://localhost:8080/api/v1.0/pizzas");
  pizzas.value = data.data;
};
const openPizza = (id) => {
  pizzas.value.forEach((pizza) => {
    if (pizza.id === id) {
      pizzaActive.value = pizza;
    }
  });
};
const pizzaCreated = () => {
  creatingPizza.value = false;
  getPizzas();
};
const deletePizza = () => {
  pizzaActive.value = null;
  getPizzas();
};
const closePage = (update) => {
  pizzaActive.value = null;
  creatingPizza.value = false;
  getPizzas();
};

// HOOKS
onMounted(getPizzas);
</script>

<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12" v-if="creatingPizza">
        <pizza-form @close-page="closePage" @created="pizzaCreated" />
      </div>
      <div class="col-12" v-else>
        <pizza-index v-if="pizzaActive == null" :pizzas="pizzas" @open-pizza="openPizza"
          @create-pizza="creatingPizza = true" />
        <pizza-show v-else :pizza="pizzaActive" @close-page="closePage" @delete-pizza="deletePizza" />
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@use './styles/generals.scss' as *;
</style>