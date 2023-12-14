<script setup>
// IMPORT LIBS
import { ref } from 'vue';
import { computed } from 'vue';
import { defineProps } from 'vue';

// EMITS
const emits = defineEmits(["openPizza"]);

// PROPS
const props = defineProps({
    pizzas: {
        type: Array,
        required: true
    }
});

// DATA
const nomeFilter = ref('');

// COMPUTED PROPERTY
const filteredPizzas = computed(() => {
    const searchTerm = nomeFilter.value.toLowerCase().trim();
    return searchTerm ? props.pizzas.filter(pizza => pizza.nome.toLowerCase().includes(searchTerm)) : props.pizzas;
});
</script>

<template>
    <form class="index-filter-form py-5" @submit.prevent="filterPizzas">
        <input type="text" placeholder="Filtra le pizze per nome" class="form-control d-inline-block" v-model="nomeFilter">
    </form>
    <table class="table table-light table-hover index-table">
        <thead>
            <tr>
                <th scope="col">ID</th>
                <th scope="col">Nome</th>
                <th scope="col">Strumenti</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="pizza in filteredPizzas" :key="pizza.id">
                <th scope="row" v-text="pizza.id"></th>
                <td v-text="pizza.nome"></td>
                <td>
                    <button class="btn btn-info" @click="$emit('openPizza', pizza.id)">
                        <i class="fas fa-eye"></i>
                    </button>
                </td>
            </tr>
            <tr>
                <td colspan="3" class="py-4">
                    <button class="btn btn-success" @click="$emit('createPizza')">Crea</button>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<style lang="scss" scoped>
@use '../styles/generals.scss' as *;

.index-filter-form {
    display: flex;
    justify-content: center;
    background-color: white;

    input {
        width: 75%;
        color: black !important;
        border: 1px solid black;
    }
}

.index-table {
    width: 50vw !important;
}
</style>