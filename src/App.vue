<script lang="ts">
import { PlusIcon } from '@heroicons/vue/24/solid';
import ExpenseItem from './components/ExpenseItem.vue';
export default {
  data() {
    return {
      expenses: [
        {
          id: '1',
          total: 15,
          description: 'iphone 14 pro',
          edit: false,
        },
      ],
    };
  },
  components: { PlusIcon, ExpenseItem },
  methods: {
    addExpense() {
      const newExpense = {
        id: crypto.randomUUID(),
        total: 0,
        description: '',
        edit: true,
      };
      this.expenses.push(newExpense);
    },
    saveExpense(updatedExpense: any) {
      this.expenses = this.expenses.map((expense) =>
        expense.id === updatedExpense.id ? { ...updatedExpense, edit: false } : expense,
      );
      window.localStorage.setItem('expenses', JSON.stringify(this.expenses));
    },
    removeExpense(id: string) {
      console.log(
        'id',
        this.expenses.filter((expense) => expense.id !== id),
      );
      this.expenses = this.expenses.filter((expense) => expense.id !== id);
      window.localStorage.setItem('expenses', JSON.stringify(this.expenses));
    },
    calculateTotal() {
      return this.expenses.reduce((acc, expense) => acc + expense.total, 0).toLocaleString('en-US');
    },
  },
  mounted() {
    const expenses = window.localStorage.getItem('expenses');
    if (expenses) return (this.expenses = JSON.parse(expenses));
  },
};
</script>

<template>
  <div class="relative flex min-h-screen flex-col justify-center overflow-hidden py-6 sm:py-12">
    <div
      class="relative bg-slate-600 px-6 pt-10 pb-8 shadow-xl ring-1 ring-gray-900/5 sm:mx-auto sm:max-w-lg sm:rounded-lg sm:px-10"
    >
      <div class="mx-auto max-w-md">
        <div class="divide-y divide-gray-300/50">
          <div class="space-y-6 py-8 text-base leading-7">
            <div class="flex justify-end">
              <button @click="addExpense()" class="btn btn-success flex justify-around">
                <PlusIcon class="h-6 w-6" />
                <span> Add Expense </span>
              </button>
            </div>
            <ul :key="expense.id" v-for="expense in expenses">
              <ExpenseItem
                @save-expense="saveExpense"
                @remove-expense="removeExpense"
                :id="expense.id"
                :edit="expense.edit"
                :description="expense.description"
                :total="expense.total"
              />
            </ul>
            <div class="divider"></div>
            <div class="grid grid-cols-3 gap-2">
              <span class="text-xl text-white">Total</span>
              <span class="text-xl text-white">${{ calculateTotal() }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
