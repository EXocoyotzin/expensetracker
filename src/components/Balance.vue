<template>
    <div>
        <h1>Tu saldo es:</h1>
        <h2 id="balance">${{total}}</h2>
    </div>
</template>
<script setup>
import { defineProps } from 'vue';
const props = defineProps({
    total: {
        type: Number,
        required: true,
    }
});
</script>