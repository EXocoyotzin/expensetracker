<template>
    <div>
        <h3>Agregar nueva transacción</h3>
        <form id="form" @submit.prevent="onSubmit">
            <div class="form-control">
                <label for="text">Text</label>
                <input type="text" id="text" v-model="text" placeholder="Enter text...">
            </div>
            <div class="form-control">
                <label for="amount">Cantidad <br>
                    (negative - expense, positive -income)</label>
                <input type="text" id="amount" v-model="amount" placeholder="Ingrese cantidad">
            </div>
            <button class="btn">Agregar </button>
        </form>
    </div>
</template>
<script setup>
import { ref } from 'vue';
import { useToast } from 'vue-toastification';
const text = ref('');
const amount = ref(0);
const emit = defineEmits(['transactionSubmitted']);
const toast = useToast();
const onSubmit = () => {
    if(!text.value || !amount.value){
        toast.error('Rellene ambos campos antes de enviar');
        return;
    }
    const transactionData = {
        text: text.value,
        amount: parseFloat(amount.value)
    };
    emit('transactionSubmitted', transactionData);
    text.value = '';
    amount.value = 0;
    
}
</script>