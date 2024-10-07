<template>
  <div class="transaction-list">
    <h2>Historial de Transacciones</h2>
    <ul>
      <li v-for="transaction in transactions" :key="transaction.id">
        <span>{{ transaction.description }}</span>
        <span :class="transaction.type">{{ formatCurrency(transaction.amount) }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['transactions'],
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
