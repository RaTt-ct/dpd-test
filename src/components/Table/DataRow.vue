<template>
  <img :src="data.picture.medium" class="rounded-full mx-auto hover:scale-[1.2] transition duration-300 ease-in-out " />
  <span>{{ `${data.name.title} ${data.name.first} ${data.name.last}` }}</span>
  <span>{{ data.gender }}</span>
  <span>{{ data.location.country }}</span>
  <span>{{ moment(data.dob.date).format("DD-MM-YYYY") }}</span>
  <span>{{ data.email }}</span>
  <span>{{ data.phone }}</span>
</template>

<script setup lang="ts">
import moment from "moment";

type DataProp = {
  gender: string;
  name: {
    title: string;
    first: string;
    lase: string;
  };
  location: {
    street: {
      number: number;
      name: string;
    };
    city: string;
    state: string;
    country: string;
    postcode: number;
    coordinates: {
      latitude: string;
      longitude: string;
    };
    timezone: {
      offset: string;
      description: string;
    };
  };
  email: string;
  login: {
    uuid: string;
    username: string;
    password: string;
    salt: string;
    md5: string;
    sha1: string;
    sha256: string;
  };
  dob: {
    date: string;
    age: number;
  };
  registrered: {
    date: string;
    age: number;
  };
  phone: string;
  cell: string;
  id: {
    name: string;
    value: string;
  };
  picture: {
    large: string;
    medium: string;
    thumbnail: string;
  };
  nat: string;
};

const props = defineProps<{
  data: {
    type: DataProp;
    required: true;
  };
}>();
</script>
