<template>
  <div class="form-container">
    <h2>Agregar Transacción</h2>
    <form @submit.prevent="addTransaction">
      <div>
        <label>Descripción</label>
        <input type="text" v-model="description" required />
      </div>
      <div>
        <label>Monto</label>
        <input type="number" v-model="amount" required />
      </div>
      <div>
        <label>Tipo</label>
        <select v-model="type" required>
          <option value="income">Ingreso</option>
          <option value="expense">Egreso</option>
        </select>
      </div>
      <button type="submit">Agregar</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      description: '',
      amount: 0,
      type: 'income',
    };
  },
  methods: {
    addTransaction() {
      const transaction = {
        id: Date.now(),
        description: this.description,
        amount: parseFloat(this.amount),
        type: this.type,
      };
      this.$emit('add-transaction', transaction);
      this.description = '';
      this.amount = 0;
      this.type = 'income';
    },
  },
};
</script>

<style scoped>
.form-container {
  margin: 20px;
}
</style>
