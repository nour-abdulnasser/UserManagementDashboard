<script setup>
// table headers
const headers = [
  { title: "Name", key: "name" },
  { title: "Email", key: "email" },
  { title: "Company", key: "company.name" },
  { title: "Phone", key: "phone" },
  { title: "Website", key: "website" },
];

const { data, status, loading, error } = await useFetch(
  "https://jsonplaceholder.typicode.com/users"
);

const itemsPerPage = ref(5);
const serverItems = data;
const search = ref("");
const totalItemsNum = ref(data.length);


console.log(status);
</script>

<template>
  <div>
    <v-data-table-server
      :items-per-page="itemsPerPage"
      :headers="headers"
      :items="serverItems"
      :items-length="totalItemsNum"
      :loading="loading"
      :search="search"
      item-value="name"
      @update:options="loadItems"
    ></v-data-table-server>
  </div>
</template>