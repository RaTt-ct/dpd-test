<template>
  <div class="flex justify-end mr-4 mb-4">
    <input
      type="text"
      name="search"
      class="border border-solid rounded-2xl pl-6 py-2 text-2xl"
      placeholder="Поиск..."
      :value="searchValue"
      @input="(ev) => $emit('search', ev.target.value, true)"
    />
  </div>
</template>

<script lang="ts" setup>
import { watch, ref } from "vue";

const props = defineProps<{
  searchQuery: string;
}>();

const searchValue = ref(props.searchQuery);

watch(props, () => {
  searchValue.value = props.searchQuery;
});
</script>
