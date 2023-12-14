<script setup>
// IMPORT LIBS
import { defineEmits, ref } from 'vue';
import axios from 'axios';

// PROPS
const props = defineProps({
    oldPizza: {
        type: Object,
        required: false,
        default: null
    }
});

// DATA
const newPizza = ref({
    prezzo: 7,
    nome: "new pizza",
    descrizione: "my new pizza desc",
    url_foto: "https://picsum.photos/300?random=10"
});
if (props.oldPizza != null) {

    newPizza.value = {
        prezzo: props.oldPizza.prezzo,
        nome: props.oldPizza.nome,
        descrizione: props.oldPizza.descrizione,
        url_foto: props.oldPizza.url_foto
    };
}

// EMITS
const emits = defineEmits(["created"]);

// FUNCTIONS
const submit = async () => {
    if (props.oldPizza == null) {

        const data = await axios.post(
            "http://localhost:8080/api/v1.0/pizzas",
            newPizza.value
        );

        emits("created");

        return;
    }

    const data = await axios.put(
        `http://localhost:8080/api/v1.0/pizzas/${props.oldPizza.id}`,
        newPizza.value
    );

    emits("created", newPizza.value);
}
</script>

<template>
    <div class="main-background text-start p-5">
        <button type="button" class="btn btn-black mb-5" @click="$emit('closePage')">
            <i class=" fa-solid fa-circle-arrow-left fa-xl"></i>
        </button>
        <form @submit.prevent="submit" class="bg-black text-center">
            <div class="mb-3">
                <label for="prezzo" class="mx-3">Prezzo</label>
                <input type="number" step="0,01" name="prezzo" id="prezzo" v-model="newPizza.prezzo">
            </div>
            <div class="mb-3">
                <label for="nome" class="mx-3">Nome</label>
                <input type="text" name="nome" id="nome" v-model="newPizza.nome">
            </div>
            <div class="mb-3">
                <label for="descrizione" class="mx-3">Descrizione</label>
                <input type="text" name="descrizione" id="descrizione" v-model="newPizza.descrizione">
            </div>
            <div class="mb-3">
                <label for="url_foto" class="mx-3">Url Photo</label>
                <input type="text" name="url_foto" id="url_foto" v-model="newPizza.url_foto">
            </div>
            <button type="submit" class="btn btn-primary my-3">{{ props.oldPizza == null ? "Crea" : "Modifica" }}</button>
        </form>
    </div>
</template>

<style lang="scss" scoped>
@use '../styles/generals.scss' as *;
</style>