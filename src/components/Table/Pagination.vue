<template>
  <div v-if="tableLength / pageSize > 1" class="mr-4">
    <div class="flex gap-2 justify-end mt-4">
      <div class="flex gap-2 items-center">
        <span>Перейти на:</span>
        <input
          min="1"
          :max="Math.ceil(tableLength / pageSize) + 1"
          type="number"
          name="pagination"
          class="h-10 w-20 pl-4 border border-solid rounded-xl flex items-center justify-center cursor-pointer"
          @input="(ev) => $emit('setPage', parseInt(ev.target.value) - 1)"
          @keyup.enter="$emit('goToPage')"
        />
      </div>
      <button
        v-if="page !== 0"
        class="h-10 w-10 border border-solid rounded-xl flex items-center justify-center cursor-pointer hover:border-[#bd0033]"
        @click="$emit('setPage', 0)"
      >
        <img src="@/assets/icons/double-arrow-left.svg" alt="Первая страница" />
      </button>
      <button
        v-if="page !== 0"
        class="h-10 w-10 border border-solid rounded-xl flex items-center justify-center cursor-pointer hover:border-[#bd0033]"
        @click="$emit('setPage', page - 1)"
      >
        {{ page }}
      </button>
      <button
        v-if="
          page !== Math.ceil(tableLength / pageSize) &&
          page < Math.ceil(tableLength / pageSize) - 1
        "
        class="h-10 w-10 border border-solid rounded-xl flex items-center justify-center cursor-pointer hover:border-[#bd0033]"
        @click="$emit('setPage', page + 1)"
      >
        {{ page + 2 }}
      </button>
      <button
        v-if="
          page !== Math.ceil(tableLength / pageSize) &&
          page < Math.ceil(tableLength / pageSize) - 1
        "
        class="h-10 w-10 border border-solid rounded-xl flex items-center justify-center cursor-pointer rotate-180 hover:border-[#bd0033]"
        @click="$emit('setPage', Math.ceil(tableLength / pageSize) - 1)"
      >
        <img
          src="@/assets/icons/double-arrow-left.svg"
          alt="Последняя страница"
        />
      </button>
    </div>
    <div class="flex justify-end pr-4">
      <span>Всего страниц: {{ Math.ceil(tableLength / pageSize) }}</span>
    </div>
  </div>

  <div v-else-if="Math.ceil(tableLength / pageSize) == 0">
    <span class="mt-4 block">По текущему запросу результаты не найдены</span>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";

const props = defineProps<{
  page: number;
  pageSize: number;
  tableLength: number;
}>();

const page = ref(props.page);
const pageSize = ref(props.pageSize);
const tableLength = ref(props.tableLength);

watch(props, () => {
  page.value = props.page;
  pageSize.value = props.pageSize;
  tableLength.value = props.tableLength;
});
</script>
