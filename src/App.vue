<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import {ref, computed} from 'vue'

//array 
const transactions = ref([
  {id: 1, text:'Paycheck', amount: 700.00},
  {id: 2, text:'Water Bill', amount: -72.83},
  {id: 3, text:'Electic Bill', amount: -153.89},
  {id: 4, text:'Returned Item', amount: 20.00},
])

//x represents one element in the transaction
//sum is a variable used to display the total sum of the array
const sum = computed(()=>{
  return transactions.value.reduce((acc, x) =>{
    //running total 
    return acc+x.amount
  },0) 
})

const moneyIn = computed(()=>{
  return transactions.value
  .filter((x)=>x.amount>0)
  .reduce((acc, x) =>{
    //running total 
    return acc+x.amount
  },0) 
})

const moneyOut = computed(()=>{
  return transactions.value
  .filter((x)=>x.amount<0)
  .reduce((acc, x) =>{
    //running total 
    return acc+x.amount
  },0) 
})

</script>

<template>
  <Header></Header>
  <div class="container">
    <Balance :total="sum"></Balance>
    <IncomeExpense :income="moneyIn" :expense="moneyOut"></IncomeExpense>
  </div>
</template>