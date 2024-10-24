<template>
    <div>
        <h3>Historial</h3>
        <ul id="list" class="list">
            <li v-for="transaction in transactions" 
            :key="transaction.id"
            class="transaction.amount < 0 ? 'minus' 
            :'plus'" > 
            {{ transaction.text }} <span>{{ transaction.amount }} </span>
            <button @click="deleteTransaction(transaction.id)"
            class="delete-btn">x</button>
            </li>
        </ul>
    </div>
</template>

<script setup>
import { defineProps } from 'vue';

const emit = defineEmits(['transactionDeleted']); 
// Define el evento personalizado para eliminar transacciones
// lo hace dando acceso a la función deleteTransaction desde 
//el componente padre

const props = defineProps({
   transactions: {
         type: Array,
         required: true,
   } 
});
const deleteTransaction = (id) => { 
    emit('transactionDeleted', id);
    console.log(id);
}
</script>

