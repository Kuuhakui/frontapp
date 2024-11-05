<template>
    <div class="expense-planner">
      <h2>Планирование расходов</h2>
      <form @submit.prevent="handleAddPlan">
        <div class="form-group">
          <label for="category">Категория:</label>
          <input type="text" id="category" v-model="category" />
        </div>
        <div class="form-group">
          <label for="amount">Сумма:</label>
          <input type="number" id="amount" v-model="amount" />
        </div>
        <button type="submit">Добавить план</button>
      </form>
      <ul>
        <li v-for="plan in plannedExpenses" :key="plan.category">
          {{ plan.category }}: {{ plan.amount }} ₽
        </li>
      </ul>
    </div>
  </template>

<script>
import { ref } from 'vue';

export default {
  name: 'ExpensePlanner',
  props: {
    plannedExpenses: Array
  },
  emits: ['add-plan'],
  setup(props, { emit }) {
    const category = ref('');
    const amount = ref('');

    const handleAddPlan = () => {
      if (category.value && amount.value) {
        emit('add-plan', { category: category.value, amount: parseInt(amount.value) });
        category.value = '';
        amount.value = '';
      } else {
        alert('Заполните все поля');
      }
    };

    return {
      category,
      amount,
      handleAddPlan
    };
  }
};
</script>

<style scoped>
.expense-planner {
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

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}
input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #218838;
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