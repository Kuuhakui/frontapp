<template>
    <div>
      <h2>Список расходов</h2>
      <ul>
        <li v-for="expense in expenses" :key="expense.date + expense.category">
          {{ expense.date }} - {{ expense.category }}: {{ expense.amount }} ₽
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ExpenseList',
    props: {
      expenses: Array
    }
  };
  </script>
  <style scoped>
  .expense-list {
    max-width: 600px;
    margin: 20px auto;
    padding: 20px;
    background-color: #ffffff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
  }
  
  h2 {
    text-align: center;
    color: #333;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  li {
    padding: 10px;
    border-bottom: 1px solid #ccc;
  }
  </style>
  
  