<script setup>
import axios from "axios";
import { reactive } from "vue";
// import { Subject } from "rxjs";
// import { debounceTime, distinctUntilChanged, switchMap } from "rxjs/operators";

const state = reactive({
  search: "df",
  items: [
    {
      id: 1,
      name: "tushar",
      price: 50,
      description: "done dgfs",
      category: "all ctg",
      stock_quantity: 20,
      manufacture_date: "20/8/2020",
    },
    {
      id: 2,
      name: "John",
      price: 45,
      description: "Lorem ipsum",
      category: "electronics",
      stock_quantity: 15,
      manufacture_date: "10/5/2021",
    },
    {
      id: 3,
      name: "Alice",
      price: 30,
      description: "Sample description",
      category: "clothing",
      stock_quantity: 25,
      manufacture_date: "5/3/2019",
    },
  ],
  pagination: {
    sortBy: "desc",
    descending: false,
    page: 1,
    rowsPerPage: 4,
  },
  loading: false,
  columns: [
    {
      name: "id",
      required: true,
      label: "ID",
      align: "left",
      field: "id",
      sortable: true,
    },
    {
      name: "name",
      required: true,
      label: "Name",
      align: "left",
      field: "name",
      sortable: true,
    },
    {
      name: "price",
      required: true,
      label: "Price",
      align: "left",
      field: "price",
      sortable: true,
    },
    {
      name: "description",
      required: true,
      label: "Description",
      align: "left",
      field: "description",
      sortable: true,
    },
    {
      name: "category",
      required: true,
      label: "Category",
      align: "left",
      field: "category",
      sortable: true,
    },
    {
      name: "stock_quantity",
      required: true,
      label: "Stock quantity",
      align: "left",
      field: "stock_quantity",
      sortable: true,
    },
    {
      name: "manufacture_date",
      required: true,
      label: "Manufacture date",
      align: "left",
      field: "manufacture_date",
      sortable: true,
    },
  ],
});
// const searchInput$ = new Subject();

// searchInput$
//   .pipe(
//     debounceTime(300),
//     distinctUntilChanged(),
//     switchMap((searchTerm) => {
//       return axios.get(
//         `https://8ca4-117-247-48-188.ngrok-free.app/item/search`,
//         {
//           params: {
//             name: searchTerm,
//             page: state.pagination.page,
//             // rowsPerPage: state.pagination.rowsPerPage,
//             // sortBy: state.pagination.sortBy,
//             // descending: state.pagination.descending,
//           },
//         }
//       );
//     })
//   )
// .subscribe((response) => {
//   state.items = response.data;
//   state.loading = false;
// });

// watch(
//   () => state.search,
//   (newSearchTerm) => {
//     state.loading = true;
//     searchInput$.next(newSearchTerm);
//   }
// );
// onMounted(() => {
//   searchInput$.next(state.search);
// });
const searchItems = async (data) => {
  state.loading = true;
  console.log(data);
  try {
    const response = axios.get(
      `https://8ca4-117-247-48-188.ngrok-free.app/item/search`,
      {
        params: {
          name: searchTerm,
          page: state.pagination.page,
          // rowsPerPage: state.pagination.rowsPerPage,
          // sortBy: state.pagination.sortBy,
          // descending: state.pagination.descending,
        },
      }
    );
    const response = await axios.get(
      "https://jsonplaceholder.typicode.com/posts",
      {
        params: {
          // You can add query parameters here, if needed
        },
        cancelToken: cancelToken,
      }
    );

    state.items = response.data;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
  state.loading = false;
};
</script>
<template>
  <div class="q-pt-md q-px-md">
    <q-card class="">
      <div class="q-py-sm">
        <q-input
          v-model="state.search"
          label="Search"
          class=""
          dense
          @update:model-value="(text) => searchItems(text)"
        >
          <template v-slot:prepend>
            <q-icon name="search" class="q-pl-md" />
          </template>
        </q-input>
      </div>
      <q-table
        :rows="state.items"
        :rows-per-page-options="[10, 20, 30]"
        v-model:pagination="state.pagination"
        :loading="state.loading"
        :columns="state.columns"
      ></q-table>
    </q-card>
  </div>
</template>
