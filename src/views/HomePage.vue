<template>
    <div class="home-page">
      <h1>Анализ финансов</h1>
      <div class="tabs">
        <button @click="changeTab('overall')">Общая</button>
      <button @click="changeTab('incomes')">Доходы</button>
      <button @click="changeTab('expenses')">Расходы</button>
    </div>
      <div class="filters">
        <label for="filter">Фильтр:</label>
        <select v-model="filter" @change="applyFilter">
          <option value="day">День</option>
          <option value="week">Неделя</option>
          <option value="month">Месяц</option>
          <option value="year">Год</option>
        </select>
      </div>
      <div class="search">
      <label for="categorySearch">Поиск по категориям:</label>
      <input type="text" id="categorySearch" v-model="searchQuery" @input="applySearch" placeholder="Введите категорию" />
    </div>
      <div v-if="currentTab === 'overall'">
      <ExpenseChart :expenses="filteredExpenses" :incomes="filteredIncomes" />
      <div class="summary">
        <h2>Общий баланс: <span :style="{ color: netIncome >= 0 ? 'green' : 'red' }">{{ netIncome }} ₽</span></h2>
      </div>
    </div>
    <div v-if="currentTab === 'incomes'">
      <h2>Доходы</h2>
      <ExpenseChart :expenses="[]" :incomes="filteredIncomes" />
      <ExpenseList :expenses="filteredIncomes" />
    </div>
    <div v-if="currentTab === 'expenses'">
      <h2>Расходы</h2>
      <ExpenseChart :expenses="filteredExpenses" :incomes="[]" />
      <ExpenseList :expenses="filteredExpenses" />
    </div>
    <router-link to="/planner" class="planner-link">Перейти к планированию расходов</router-link>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import ExpenseChart from '../components/ExpenseChart.vue';
import ExpenseList from '../components/ExpenseList.vue';
import { useRouter } from 'vue-router';
import dayjs from 'dayjs';

export default {
  name: 'HomePage',
  components: {
    ExpenseChart,
    ExpenseList
  },
  setup() {
    const router = useRouter();

    // Проверка авторизации (тестовая логика)
    const isAuthenticated = localStorage.getItem('isAuthenticated');
    if (!isAuthenticated) {
      router.push('/');
    }

    const expenses = ref([
      { date: '2024-11-05', category: 'Супермаркеты', amount: 1194 },
      { date: '2024-11-04', category: 'ЖКХ, связь, интернет', amount: 200 },
      { date: '2024-11-03', category: 'Переводы людям', amount: 199 },
      { date: '2024-11-02', category: 'Транспорт', amount: 87 }
    ]);
    const incomes = ref([
      { date: '2024-11-05', category: 'Зарплата', amount: 5000 },
      { date: '2024-11-04', category: 'Фриланс', amount: 1500 }
    ]);

    const plannedExpenses = ref([]);
    const filter = ref('month');
    const currentTab = ref('overall');
    const searchQuery = ref('');

    const filteredExpenses = computed(() => {
        return applySearchFilter(applyDateFilter(expenses.value));
    });

    const filteredIncomes = computed(() => {
        return applySearchFilter(applyDateFilter(incomes.value));
    });

    const netIncome = computed(() => {
      const totalIncomes = filteredIncomes.value.reduce((acc, income) => acc + income.amount, 0);
      const totalExpenses = filteredExpenses.value.reduce((acc, expense) => acc + expense.amount, 0);
      return totalIncomes - totalExpenses;
    });

    const applyDateFilter = (items) => {
      const now = dayjs();
      return items.filter(item => {
        const itemDate = dayjs(item.date);
        switch (filter.value) {
          case 'day':
            return itemDate.isSame(now, 'day');
          case 'week':
            return itemDate.isSame(now, 'week');
          case 'month':
            return itemDate.isSame(now, 'month');
          case 'year':
            return itemDate.isSame(now, 'year');
          default:
            return true;
        }
      });
    };

    const applySearchFilter = (items) => {
      if (!searchQuery.value) return items;
      const queries = searchQuery.value.split(',').map(q => q.trim().toLowerCase());
      return items.filter(item => queries.some(query => item.category.toLowerCase().includes(query)));
    };


    const addPlan = (plan) => {
      plannedExpenses.value.push(plan);
    };

    const applyFilter = () => {
      // This function is just here to trigger the computed properties to re-run
    };

    const changeTab = (tab) => {
      currentTab.value = tab;
      applyFilter(); // Trigger re-computation when changing tabs
    };

    const applySearch = () => {
      applyFilter(); // Trigger re-computation when applying search
    };

    return { expenses, incomes, plannedExpenses, filter, filteredExpenses, filteredIncomes, addPlan, applyFilter, currentTab, netIncome, changeTab, searchQuery, applySearch };
  }
};
</script>
<style scoped>
.home-page {
  padding: 20px;
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
}

.filters {
  margin-bottom: 20px;
}
.search {
  margin-bottom: 20px;
}
.tabs {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.tabs button {
  padding: 10px;
  background-color: #007BFF;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.tabs button:hover {
  background-color: #0056b3;
}

.planner-link {
  display: block;
  margin-top: 20px;
  text-align: center;
  font-weight: bold;
  color: #007BFF;
  text-decoration: none;
}

.planner-link:hover {
  text-decoration: underline;
}


.summary {
  text-align: center;
  margin-top: 20px;
}

h1, h2 {
  color: #333;
}
</style>