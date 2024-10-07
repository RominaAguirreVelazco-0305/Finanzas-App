<template>
  <div class="summary">
    <h2>Resumen</h2>
    <p>Ingresos: {{ formatCurrency(income) }}</p>
    <p>Egresos: {{ formatCurrency(expenses) }}</p>
    <p>Total: {{ formatCurrency(total) }}</p>
  </div>
</template>

<script>
export default {
  props: {
    transactions: {
      type: Array,
      required: true
    }
  },
  computed: {
    income() {
      return this.transactions
        .filter(t => t.type === 'income')
        .reduce((acc, t) => acc + t.amount, 0);
    },
    expenses() {
      return this.transactions
        .filter(t => t.type === 'expense')
        .reduce((acc, t) => acc + t.amount, 0);
    },
    total() {
      return this.income - this.expenses;
    },
  },
  methods: {
    formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', {
        style: 'currency',
        currency: 'MXN',
      }).format(value);
    },
  },
};
</script>

<style scoped>
.summary {
  margin: 20px;
}
</style>
