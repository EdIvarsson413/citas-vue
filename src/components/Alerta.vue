<template>
    <div
        :class="[isError ? 'bg-red-500' : 'bg-green-700']"
        class="text-white text-center uppercase font-bold p-3 rounded-md mb-3"
    >
        {{ alerta.mensaje }}
    </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    alerta: {
        type: Object,
        required: true
    },
})

// Se evalua el tipo de mensaje, retornara un booleano
const isError = computed(() => {
    return props.alerta.tipo === 'error';
})
</script>