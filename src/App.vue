<template>
  <div class="container">
    <Header/>
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions"  
    @transactionDeleted="handleTransactionDeleted"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
    <br>
  </div>
</template>
<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { useToast } from 'vue-toastification';
import { computed, ref, onMounted } from 'vue';
const toast = useToast();
const transactions = ref([]);
onMounted(() => {
  const savedTransactions = 
  JSON.parse(localStorage.getItem('transactions'));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
})

// Obtener el total
const total = computed(()=> {
  return transactions.value.reduce((acc, transaction)=>{
    return acc + transaction.amount;
  }, 0);
});
// Obtener ingresos
const income = computed(()=>{
  return transactions.value
    .filter((transaction)=> transaction.amount > 0)
    .reduce((acc, transaction)=>{
      return acc + transaction.amount;
    }, 0)
    .toFixed(2); // Redondear a 2 decimales
})
// Obtener gastos 
const expenses = computed(()=>{
  return transactions.value
    .filter((transaction)=> transaction.amount < 0)
    .reduce((acc, transaction)=>{
      return acc + transaction.amount;
    }, 0)
    .toFixed(2); // Redondear a 2 decimales
});

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount  
  });
  toast.success('Transacción completada');
};
const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000000);
};
// Eliminar transacción. 
const handleransactionDeleted = (id) =>{ 
  transactions.value =
  transactions.value.filter((transaction) =>
  transaction.id !== id);
  toast.success('Transacción eliminada');
};

</script>