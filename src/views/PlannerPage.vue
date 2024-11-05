<template>
    <div class="planner-page">
      <h1>Планирование расходов</h1>
      <ExpensePlanner :plannedExpenses="plannedExpenses" @add-plan="addPlan" />
    </div>
  </template>

<script>
import ExpensePlanner from '../components/ExpensePlanner.vue';
import { ref } from 'vue';

export default {
  name: 'PlannerPage',
  components: {
    ExpensePlanner
  },
  setup() {
    const plannedExpenses = ref([]);

    const addPlan = (plan) => {
      plannedExpenses.value.push(plan);
    };

    return { plannedExpenses, addPlan };
  }
};
</script>

<style scoped>
.planner-page {
  padding: 20px;
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
}

h1 {
  color: #333;
}
</style>