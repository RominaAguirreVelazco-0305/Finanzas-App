<template>
  <div id="app">
    <h1>Aplicación de Finanzas Personales</h1>
    <form @submit.prevent="setUserName">
      <label for="userName">Ingresa tu nombre:</label>
      <!-- Aquí se agrega el placeholder para mostrar un ejemplo visual -->
      <input type="text" id="userName" v-model="userNameInput" placeholder="Ejemplo: Juan Pérez" />
      <button type="submit">Confirmar</button>
    </form>
    
    <div v-if="userName">
      <h2>Bienvenido, {{ userName }} 👋</h2>
    </div>

    <IncomeExpenseForm @add-transaction="addTransaction" />
    <FinanceSummary :transactions="transactions" />
    <TransactionList :transactions="transactions" />
    <canvas id="financialChart"></canvas> <!-- Gráfico de barras para mostrar ingresos y egresos -->
  </div>
</template>

<script>
import IncomeExpenseForm from './components/IncomeExpenseForm.vue';
import TransactionList from './components/TransactionList.vue';
import FinanceSummary from './components/FinanceSummary.vue';
import Chart from 'chart.js/auto';  // Importamos Chart.js

export default {
  components: {
    IncomeExpenseForm,
    TransactionList,
    FinanceSummary,
  },
  data() {
    return {
      userName: '',
      userNameInput: '', // Input para capturar el nombre del usuario
      transactions: [],
      chart: null, // Variable para almacenar el gráfico
    };
  },
  methods: {
    setUserName() {
      this.userName = this.userNameInput;
    },
    addTransaction(transaction) {
      this.transactions.push(transaction);
      this.updateChart(); // Actualizamos el gráfico cada vez que se agrega una transacción
    },
    updateChart() {
      const income = this.transactions.filter(t => t.type === 'income').reduce((acc, t) => acc + t.amount, 0);
      const expenses = this.transactions.filter(t => t.type === 'expense').reduce((acc, t) => acc + t.amount, 0);

      if (this.chart) {
        this.chart.destroy();  // Destruimos el gráfico anterior si ya existe
      }

      const ctx = document.getElementById('financialChart').getContext('2d');
      this.chart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: ['Ingresos', 'Egresos'],
          datasets: [{
            label: 'Finanzas',
            data: [income, expenses],
            backgroundColor: ['#4caf50', '#e57373'],
            borderColor: ['#388e3c', '#d32f2f'],
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    },
  },
  mounted() {
    this.updateChart();  // Inicializamos el gráfico cuando el componente se monta
  },
};
</script>

<style scoped>
  /* Fondo general de la página */
  body {
    background-color: #e8f5e9;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    margin: 0;
    padding: 0;
  }

  /* Estilo del contenedor principal */
  #app {
    max-width: 900px;
    margin: 0 auto;
    padding: 20px;
    text-align: center;
    background-color: white;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    margin-top: 30px;
  }

  /* Título principal */
  h1 {
    font-size: 2.5rem;
    color: #1b5e20;
    margin-bottom: 20px;
    font-weight: bold;
  }

  /* Emoji para título */
  h1::before {
    content: "💰 ";
  }

  /* Formulario para ingresar el nombre del usuario */
  form {
    margin-bottom: 20px;
  }

  input[type="text"] {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 1rem;
  }

  button {
    background-color: #388e3c;
    color: white;
    padding: 10px;
    border: none;
    border-radius: 5px;
    font-size: 1.1rem;
    cursor: pointer;
    transition: background-color 0.3s;
    margin-left: 10px;
  }

  button:hover {
    background-color: #2e7d32;
  }

  /* Estilo del saludo de bienvenida */
  h2 {
    color: #1b5e20;
    margin-bottom: 20px;
  }

  /* Estilo del formulario de ingresos y gastos */
  form {
    display: grid;
    gap: 10px;
    margin-bottom: 20px;
    background-color: #a5d6a7;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  /* Estilo de los campos de entrada */
  input, select {
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 1rem;
  }

  /* Estilo del resumen */
  .summary {
    margin: 20px 0;
    padding: 20px;
    background-color: #c8e6c9;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .summary p {
    font-size: 1.2rem;
    margin: 10px 0;
  }

  /* Historial de transacciones */
  .transaction-list {
    text-align: left;
    background-color: #dcedc8;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .transaction-list h3 {
    font-size: 1.5rem;
    color: #388e3c;
    margin-bottom: 15px;
  }

  .transaction-list ul {
    list-style-type: none;
    padding: 0;
  }

  .transaction-list li {
    padding: 10px;
    background-color: #a5d6a7;
    border-radius: 5px;
    margin-bottom: 10px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    font-size: 1.1rem;
    display: flex;
    justify-content: space-between;
  }

  .transaction-list li:nth-child(even) {
    background-color: #81c784;
  }

  .transaction-list li::before {
    content: "📝 ";
  }

  /* Estilos responsivos */
  @media (max-width: 768px) {
    #app {
      padding: 10px;
    }

    h1 {
      font-size: 2rem;
    }

    form, .summary, .transaction-list {
      padding: 10px;
    }

    button {
      font-size: 1rem;
    }
  }

  /* Estilo para el gráfico */
  #financialChart {
    max-width: 100%;
    margin-top: 30px;
  }
</style>
