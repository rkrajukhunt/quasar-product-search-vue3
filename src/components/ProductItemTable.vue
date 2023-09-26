<script setup>
import axios from "axios";
import { onMounted, reactive, ref } from "vue";

const tableRef = ref();

const state = reactive({
  search: "",
  items: [],
  loading: false,
  pagination: {
    sortBy: "id",
    descending: false,
    page: 0,
    rowsPerPage: 10,
    rowsNumber: 10,
  },
  columns: [
    {
      name: "id",
      required: true,
      label: "Id",
      align: "left",
      field: (row) => row.id,
      format: (val) => val,
      sortable: true,
    },
    {
      name: "name",
      align: "center",
      label: "Name",
      field: "name",
      sortable: true,
    },
    {
      name: "price",
      align: "center",
      label: "Price",
      field: "price",
      sortable: true,
    },
    {
      name: "description",
      align: "center",
      label: "Description",
      field: "description",
      sortable: true,
    },
    {
      name: "manufactureDate",
      align: "center",
      label: "ManufactureDate",
      field: "manufactureDate",
      sortable: true,
    },
    {
      name: "stockQuantity",
      align: "center",
      label: "StockQuantity",
      field: "stockQuantity",
      sortable: true,
    },
    {
      name: "category",
      align: "center",
      label: "Category",
      field: "category",
      sortable: true,
    },
  ],
});

onMounted(() => {
  tableRef.value.requestServerInteraction();
});

const searchItems = async (props) => {
  state.loading = true;
  try {
    const filter = props.filter;
    const { page, rowsPerPage, sortBy, descending } = props.pagination;
    console.log(sortBy, descending);
    const { data } = await axios.get(`http://localhost:8080/item/search`, {
      params: {
        name: filter,
        page: page - 1,
        sortBy: sortBy,
        size: rowsPerPage,
        sortDirection: descending ? "DESC" : "ASC",
      },
    });
    state.items = data.content;

    state.pagination.page = page;
    state.pagination.sortBy = sortBy;
    state.pagination.descending = descending;
    state.pagination.rowsPerPage = rowsPerPage;
    state.pagination.rowsNumber = data?.totalElements;
  } catch (error) {
    console.error("Error fetching data:", error);
  }
  state.loading = false;
};
</script>
<template>
  <div class="q-pt-md q-px-md">
    <q-card class="">
      <q-table
        ref="tableRef"
        title="Product"
        :rows="state.items"
        :rows-per-page-options="[10, 20, 30, 40, 50]"
        :loading="state.loading"
        :columns="state.columns"
        :filter="state.search"
        v-model:pagination="state.pagination"
        @request="searchItems"
      >
        <template v-slot:top-right>
          <q-input
            borderless
            dense
            debounce="500"
            v-model="state.search"
            placeholder="Search"
          >
            <template v-slot:append>
              <q-icon name="search" />
            </template>
          </q-input>
        </template>
      </q-table>
    </q-card>
  </div>
</template>
