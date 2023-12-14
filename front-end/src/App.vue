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

// FUNCTIONS
const getPizzas = async () => {
  const data = await axios.get("http://localhost:8080/api/v1.0/pizzas");
  pizzas.value = data.data;
};

// HOOKS
onMounted(getPizzas);
</script>

<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12">
        <pizza-index :pizzas="pizzas" />
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@use './styles/generals.scss' as *;
</style>