<script lang="ts">
import { ArchiveBoxXMarkIcon, PencilSquareIcon, CheckIcon } from '@heroicons/vue/24/solid';

export default {
  data() {
    return {
      expenseId: this.id,
      desc: this.description,
      price: this.total,
      editing: this.edit,
    };
  },
  methods: {
    test(editing: boolean) {
      if (editing) {
        console.log('saved');
        const updatedExpense = {
          id: this.id,
          description: this.desc,
          total: this.price,
          edit: editing,
        };
        this.$emit('save-expense', updatedExpense);
      }
      this.editing = !this.editing;
    },
    updateExpense(e: Event) {
      const targetInput = e.target as HTMLInputElement;
      if (targetInput.name === 'desc') {
        this.desc = targetInput.value;
        return;
      }
      this.price = parseFloat(targetInput.value);
    },
  },
  props: {
    id: {
      type: String,
      required: true,
    },
    description: String,
    total: Number,
    edit: Boolean,
  },
  components: { ArchiveBoxXMarkIcon, PencilSquareIcon, CheckIcon },
  emits: ['save-expense', 'remove-expense'],
};
</script>
<template>
  <div class="grid grid-cols-3 gap-2">
    <input
      v-if="editing"
      type="text"
      placeholder="Type Here"
      class="input input-bordered input-accent w-full max-w-xs col-span-1"
      @input="updateExpense($event)"
      :value="desc"
      name="desc"
    />
    <div
      v-else
      class="w-full max-w-xs col-span-1 border border-neutral-content rounded-lg align-center flex px-4 py-2 text-white"
    >
      {{ desc }}
    </div>
    <input
      v-if="editing"
      type="number"
      name="price"
      placeholder="Type here"
      class="input input-bordered input-accent w-full max-w-xs col-span-1"
      @input="updateExpense($event)"
      :value="price"
    />
    <div
      v-else
      class="w-full max-w-xs col-span-1 border border-neutral-content rounded-lg align-center flex px-4 py-2 text-white"
    >
      ${{ price }}
    </div>
    <div class="col-span-1 flex justify-around">
      <button @click="test(editing)" class="btn btn-accent btn-circle btn-outline">
        <CheckIcon v-if="editing" class="h-6 w-6" />
        <PencilSquareIcon v-else class="h-6 w-6" />
      </button>
      <button @click="$emit('remove-expense', expenseId)" class="btn btn-error btn-circle btn-outline">
        <ArchiveBoxXMarkIcon class="h-6 w-6" />
      </button>
    </div>
  </div>
</template>
