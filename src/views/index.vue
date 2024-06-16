<template>
  <div
    class="max-h-screen max-w-[100vw] h-screen w-full flex flex-col items-center justify-center main-container"
  >
    <img src="@/assets/logo.png" alt="dpd Лого" />
    <div>
      <TableSearch @search="search" :searchQuery="searchQuery" />
      <div
        class="table-wrapper max-w-screen-2xl border border-solid max-h-[70vh] h-auto w-full mx-4 2xl:mx-0 rounded-2xl overflow-hidden overflow-x-auto overflow-y-auto relative"
      >
        <div
          class="grid grid-cols-7 py-1 bg-[#ecd0d6] rounded-t-2xl sticky top-0"
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
      <TablePagination
        :page="page"
        :pageSize="pageSize"
        :tableLength="dataFromApi.length"
        @setPage="setPage"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import apiData from "../../api/api";
import { computed, onBeforeMount, onBeforeUnmount, ref } from "vue";
import TableDataRow from "../components/Table/DataRow.vue";
import TablePagination from "../components/Table/Pagination.vue";
import TableSearch from "../components/Table/Search.vue";
import type { IApi } from "@/interfaces/api";
import { useRouter, useRoute } from "vue-router";

const pageSize = ref<number>(20);
const page = ref<number>(0);
const router = useRouter();
const route = useRoute();

const searchQuery = ref("");

const dataFromApi = ref<Array<IApi>>(apiData.results);

const tableHeaders = [
  { label: "Аватар", key: "picture.medium" },
  { label: "ФИО", key: "name.first" },
  { label: "Пол", key: "gender" },
  { label: "Страна", key: "location.country" },
  { label: "Дата рождения", key: "dob.date" },
  { label: "Адрес электронной почты", key: "email" },
  { label: "Телефон", key: "phone" },
];

const dataToGrid = computed(() =>
  dataFromApi.value.slice(
    page.value * pageSize.value,
    (page.value + 1) * pageSize.value
  )
);

const search = (query: string, setFirstPage: boolean) => {
  const filteredData: Array<IApi> = [];
  searchQuery.value = query;
  apiData.results.forEach((el) => {
    if (
      JSON.stringify(el).toLowerCase().includes(searchQuery.value.toLowerCase())
    ) {
      filteredData.push(el);
    }
  });
  dataFromApi.value = filteredData;
  if (setFirstPage) page.value = 0;
  router.push(`?query=${searchQuery.value}&page=${page.value}`);
};

const resolvePath = (object: any, path: string): string =>
  path.split(".").reduce((o, k) => (o || {})[k], object);

const sortBy = (key: string) => {
  dataFromApi.value = dataFromApi.value.sort((a, b) =>
    resolvePath(a, key) > resolvePath(b, key) ? 1 : -1
  );
};

const setPage = (pageToSet: number) => {
  if (
    pageToSet < Math.ceil(dataFromApi.value.length / pageSize.value) &&
    pageToSet >= 0
  ) {
    page.value = pageToSet;
  }
  router.push(`?query=${searchQuery.value}&page=${page.value}`);
};

onBeforeMount(() => {
  page.value = route.query.page ? parseInt(route.query.page) : 0;
  searchQuery.value = route.query.query ? route.query.query : "";
  if (searchQuery.value) search(searchQuery.value);
});
</script>

<style scoped>
.main-container {
  background-image: url("@/assets/dpd-logo-700.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: 75%;
}

.table-row:nth-child(odd) {
  background: rgba(160, 19, 52, 0.2);
}

.table-wrapper {
  background: rgba(255, 255, 255, 0.8);
}

.table-wrapper::-webkit-scrollbar {
  width: 12px;
}

.table-wrapper::-webkit-scrollbar-track {
  display: none;
}

.table-wrapper::-webkit-scrollbar-thumb {
  width: 8px;
  background: rgba(160, 19, 52, 0.2);
  border-radius: 24px;
}
</style>
