<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpense.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import TransactionList from './components/TransactionList.vue';
  import {ref, computed, onMounted} from 'vue'

  //array 
  const transactions = ref([])

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
  
  const handleTransaction = (transactionData) => {
    transactions.value.push({
      text: transactionData.text,
      amount: transactionData.amount,
    })
    saveToLocalStorage()
  }

  //setting up the x to delete the lists of expenses
  const handleDelete = (id) => {
    transactions.value = transactions.value.filter((x) => x.id !== id)
    saveToLocalStorage()
  }

  const saveToLocalStorage = () =>{
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }

  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

    if(savedTransactions){
      transactions.value = savedTransactions
    }
  })


 
</script>

<template>
  <Header></Header>
  <div class="container">
    <Balance :total="sum"></Balance>
    <IncomeExpense :income="moneyIn" :expense="moneyOut"></IncomeExpense>
    <AddTransaction @transactionSubmitted="handleTransaction"></AddTransaction>
    <TransactionList :transactions="transactions" @transactionDeleted="handleDelete"></TransactionList>
  </div>


</template>