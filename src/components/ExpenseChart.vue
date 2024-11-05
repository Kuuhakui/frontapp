<template>
    <div class="expense-chart">
      <h2>Круговая диаграмма расходов и доходов</h2>
      <canvas id="expenseChart"></canvas>
    </div>
  </template>
  <script>
  import { onMounted, watch } from 'vue';
  import { Chart, registerables } from 'chart.js';
  
  Chart.register(...registerables);
  
  export default {
    name: 'ExpenseChart',
    props: {
      expenses: Array,
      incomes: Array
    },
    setup(props) {
        let chartInstance = null;
        const createChart = () => {
        const categories = props.expenses.reduce((acc, expense) => {
          acc[expense.category] = (acc[expense.category] || 0) + expense.amount;
          return acc;
        }, {});
  
        const incomeCategories = props.incomes.reduce((acc, income) => {
          acc[income.category] = (acc[income.category] || 0) + income.amount;
          return acc;
        }, {});const data = {
        labels: [...Object.keys(categories), ...Object.keys(incomeCategories)],
        datasets: [{
          data: [...Object.values(categories), ...Object.values(incomeCategories)],
          backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#8E44AD', '#2ECC71'],
        }]
      };

      if (chartInstance) {
        chartInstance.destroy();
      }

      chartInstance = new Chart(document.getElementById('expenseChart'), {
        type: 'doughnut',
        data,
      });
    };

    onMounted(() => {
      createChart();
    });

    watch(() => [props.expenses, props.incomes], createChart, { deep: true });
  }
};
</script>

<style scoped>
.expense-chart {
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
</style>