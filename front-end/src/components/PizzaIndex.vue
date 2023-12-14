<script setup>
// PROPS
const props = defineProps({
    pizzas: {
        type: Array,
        required: true
    }
});
</script>

<template>
    <ul>
        <li v-for="pizza in pizzas" :key="pizza.id">
            {{ pizza.nome }}
        </li>
    </ul>
</template>

<style scoped></style>