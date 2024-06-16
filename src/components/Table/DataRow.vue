<template>
  <img
    :src="data.picture.medium"
    class="rounded-full mx-auto hover:scale-[1.2] transition duration-300 ease-in-out"
  />
  <span>{{ `${data.name.title} ${data.name.first} ${data.name.last}` }}</span>
  <span>{{ data.gender }}</span>
  <span>{{ data.location.country }}</span>
  <span>{{ moment(data.dob.date).format("DD-MM-YYYY") }}</span>
  <span>{{ data.email }}</span>
  <span>{{ data.phone }}</span>
</template>

<script setup lang="ts">
import type { IApi } from "@/interfaces/api";
import moment from "moment";

const props = defineProps<{
  data: IApi;
}>();
</script>
