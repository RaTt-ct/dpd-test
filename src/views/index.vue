<template>
  <div
    class="max-h-screen max-w-screen h-screen w-full flex flex-col items-center justify-center"
  >
    <div>
      <div class="flex justify-end mr-4 mb-4">
        <input
          type="text"
          name="search"
          class="border border-solid rounded-2xl pl-6 py-2 text-2xl"
          placeholder="Поиск..."
          @input="(ev) => search(ev)"
        />
      </div>

      <div
        class="table-wrapper max-w-screen-2xl border border-solid max-h-[70vh] h-auto w-full mx-4 2xl:mx-0 rounded-2xl overflow-hidden overflow-x-auto overflow-y-auto relative"
      >
        <div
          class="grid grid-cols-7 py-1 bg-[#e5e5e5] rounded-t-2xl sticky top-0"
        >
          <div
            v-for="(header, index) in tableHeaders"
            :key="index"
            class="text-center cursor-pointer"
            @click="sortBy(header.key)"
          >
            <span> {{ header.label }}</span>
          </div>
        </div>
        <div
          v-for="(data, index) in dataToGrid"
          :key="index"
          class="grid grid-cols-7 py-4 table-row text-center"
        >
          <TableDataRow :data="data" />
        </div>
      </div>

      <div
        class="flex gap-2 justify-end mr-4 mt-4"
        v-if="dataFromApi.length / pageSize > 0"
      >
        <div>
          <input
            min="1"
            :max="Math.floor(dataFromApi.length / pageSize)"
            type="number"
            name="pagination"
            placeholder="Перейти на ..."
            class="h-10 w-40 pl-4 border border-solid rounded-xl flex items-center justify-center cursor-pointer"
            v-model="paginationInput"
            @input="(ev) => (paginationInput = parseInt(ev.target.value))"
            @keyup.enter="(ev) => goToPage(ev)"
          />
        </div>
        <button
          v-if="page !== 0"
          class="h-10 w-10 border border-solid rounded-xl flex items-center justify-center cursor-pointer"
          @click="page = 0"
        >
          <img
            src="@/assets/icons/double-arrow-left.svg"
            alt="Первая страница"
          />
        </button>
        <button
          v-if="page !== 0"
          class="h-10 w-10 border border-solid rounded-xl flex items-center justify-center cursor-pointer"
          @click="page = page - 1"
        >
          {{ page }}
        </button>
        <button
          v-if="
            page !== Math.floor(dataFromApi.length / pageSize) &&
            page < Math.floor(dataFromApi.length / pageSize)
          "
          class="h-10 w-10 border border-solid rounded-xl flex items-center justify-center cursor-pointer"
          @click="page = page + 1"
        >
          {{ page + 2 }}
        </button>
        <button
          v-if="
            page !== Math.floor(dataFromApi.length / pageSize) &&
            page < Math.floor(dataFromApi.length / pageSize)
          "
          class="h-10 w-10 border border-solid rounded-xl flex items-center justify-center cursor-pointer rotate-180"
          @click="page = Math.floor(dataFromApi.length / pageSize)"
        >
          <img
            src="@/assets/icons/double-arrow-left.svg"
            alt="Последняя страница"
          />
        </button>
      </div>
      <div v-else>
        <span class="mt-4 block pl-4"
          >По текущему запросу результаты не найдены</span
        >
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import apiData from "../../api/api";
import { computed, ref, watch } from "vue";
import TableDataRow from "../components/Table/DataRow.vue";

const pageSize = ref<number>(20);
const page = ref<number>(0);
const paginationInput = ref<number>(0);

const dataFromApi = ref(apiData.results);

const tableHeaders = [
  { label: "Аватар", key: "[picture][medium]" },
  { label: "ФИО", key: "[name][first]" },
  { label: "Пол", key: "[gender]" },
  { label: "Страна", key: "[location][country]" },
  { label: "Дата рождения", key: "dob.date" },
  { label: "Адрес электронной почты", key: "[email]" },
  { label: "Телефон", key: "phone" },
];

const dataToGrid = computed(() =>
  dataFromApi.value.slice(
    page.value * pageSize.value,
    (page.value + 1) * pageSize.value
  )
);

const search = (ev: Event) => {
  const filteredData = [];
  apiData.results.forEach((el) => {
    if (
      JSON.stringify(el).toLowerCase().includes(ev.target.value.toLowerCase())
    ) {
      filteredData.push(el);
    }
  });
  dataFromApi.value = filteredData;
  page.value = 0;
};

const goToPage = () => {
  if (
    paginationInput.value - 1 < dataFromApi.value.length / pageSize.value &&
    paginationInput.value > 0
  ) {
    page.value = paginationInput.value - 1;
  }
};

const sortBy = (key) => {
  console.log(key);
  dataFromApi.value = dataFromApi.value.sort((a, b) =>
    a[key] > b[key] ? 1 : -1
  );
  console.log(dataFromApi.value);
};
</script>

<style scoped>
.table-row:nth-child(odd) {
  background: rgba(0, 0, 0, 0.1);
}

.table-wrapper::-webkit-scrollbar {
  width: 12px;
}

.table-wrapper::-webkit-scrollbar-track {
  display: none;
}

.table-wrapper::-webkit-scrollbar-thumb {
  width: 8px;
  background: rgba(0, 0, 0, 0.2);
  border-radius: 24px;
}
</style>
