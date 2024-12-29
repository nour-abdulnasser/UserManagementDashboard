<script setup>
// Columns
const headers = [
  { title: "Name", key: "name" },
  { title: "Email", key: "email" },
  { title: "Company", key: "company.name" },
  { title: "Phone", key: "phone" },
];

// Props to use with Vuetify component
const itemsPerPage = ref(5);
const page = ref(1);
const search = ref("");

// API data variables Implementing useFetch
let { data, loading } = await useFetch(
  "https://jsonplaceholder.typicode.com/users"
);

const itemsLength = computed(() => data.value.length);

// Displaying the data
const dataToDisplay = computed(() => {
  if (!data.value) return [];

  // Getting the indices of each page
  // slice method: endIndex is excluded
  // 5 items per page --> 0:4 --> slice(0, 5)
  // next page starts from: number of pages passed * items per page
  const startIndex = (page.value - 1) * itemsPerPage.value;
  const endIndex = startIndex + itemsPerPage.value;

  return data.value.slice(startIndex, endIndex);
});

// Reload to paginate
const loadItems = (options) => {
  loading = true;
  if (options.page) {
    page.value = options.page;
  }

  if (options.itemsPerPage) {
    itemsPerPage.value = options.itemsPerPage;
  }
  loading = false;
};
</script>

<template>
  <div>
    <v-data-table-server
      :headers="headers"
      :page="page"
      :items-per-page="itemsPerPage"
      :loading="loading"
      item-value="name"
      :search="search"
      :items-length="itemsLength"
      :items="dataToDisplay"
      :items-per-page-options="[5, 10, 15, 20, 25, -1]"
      @update:options="loadItems"
    >
    <template v-slot:tfoot>
      <tr>
        <td>
          <v-text-field
            v-model="search"
            class="ma-2"
            density="compact"
            placeholder="Search name..."
            hide-details
          ></v-text-field>
        </td>
      </tr>
    </template>
    </v-data-table-server>
   
  </div>
</template>



<!-- Search Template -->
<!-- <template v-slot:tfoot>
  <tr>
    <td>
      <v-text-field
        v-model="name"
        class="ma-2"
        density="compact"
        placeholder="Search name..."
        hide-details
      ></v-text-field>
    </td>
  </tr>
</template> -->
