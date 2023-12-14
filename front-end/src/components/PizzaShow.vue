<script setup>
// IMPORT LIBS
import { ref } from 'vue';
import axios from 'axios';

// IMPORT COMPONENTS
import PizzaForm from './PizzaForm.vue';

// PROPS
const props = defineProps({
    pizza: {
        type: Object,
        required: true
    }
});

// DATA
const pizzaUpdate = ref(false);
const myPizza = ref(props.pizza);

// EMITS
const emits = defineEmits(["deletePizza"]);

// FUNCTIONS
const updatedPizza = (newPizza) => {
    pizzaUpdate.value = false;
    myPizza.value = newPizza;
}
const deletePizza = async (id) => {
    const data = await axios.delete(
        `http://localhost:8080/api/v1.0/pizzas/${id}`
    );

    emits("deletePizza");
}
</script>

<template>
    <div class="main-background" v-if="!pizzaUpdate">
        <div class="text-start py-5 px-2">
            <button class="btn btn-black mx-2" @click="$emit('closePage')">
                <i class=" fa-solid fa-circle-arrow-left fa-xl"></i>
            </button>
        </div>
        <img :src="myPizza.url_foto" class="rounded">
        <div class="p-3">
            <h5 v-text="myPizza.nome" class="card-title my-3"></h5>
            <p v-text="myPizza.descrizione" class="card-text my-3"></p>
            <span v-text="myPizza.prezzo" class="d-block my-3"></span>
        </div>
        <div class="p-3">
            <button class="btn btn-warning mx-2" @click="pizzaUpdate = true">
                <i class="fas fa-edit"></i>
            </button>
            <button class="btn btn-danger mx-2" @click="deletePizza(myPizza.id)">
                <i class="fas fa-trash"></i>
            </button>
        </div>
    </div>
    <pizza-form v-else :oldPizza="pizza" @created="updatedPizza(newPizza)" />
</template>

<style lang="scss" scoped>
@use '../styles/generals.scss' as *;
</style>