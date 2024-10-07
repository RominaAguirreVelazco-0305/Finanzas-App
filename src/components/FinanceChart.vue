<template>
  <div>
    <BarChart :chart-data="datacollection" :options="options" />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'FinanceChart',
  components: {
    BarChart: Bar,
  },
  props: ['transactions'],
  data() {
    return {
      datacollection: {
        labels: ['Ingresos', 'Egresos'],
        datasets: [
          {
            label: 'Transacciones',
            backgroundColor: ['#4caf50', '#e53935'],
            data: [0, 0], // Inicializamos en 0
          },
        ],
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        scales: {
          y: {
            beginAtZero: true,
          },
        },
      },
    }
  },
  watch: {
    transactions: {
      handler(newTransactions) {
        this.updateChartData(newTransactions)
      },
      immediate: true,
      deep: true,
    },
  },
  methods: {
    updateChartData(transactions) {
      const ingresos = transactions
        .filter((t) => t.type === 'income')
        .reduce((acc, t) => acc + t.amount, 0)
      const egresos = transactions
        .filter((t) => t.type === 'expense')
        .reduce((acc, t) => acc + t.amount, 0)

      this.datacollection.datasets[0].data = [ingresos, egresos]
    },
  },
}
</script>

<style scoped>
.chart-container {
  position: relative;
  height: 400px;
  margin-bottom: 20px;
}
</style>
