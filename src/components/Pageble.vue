<template>
  <div>
    <h1>Ajax data source table</h1>

    <DataTable
      :columns="columns"
      id="example"
      :ajax="{
        url: apiUrl,
        dataSrc: 'content',
        data: fetchData,
        dataFilter: function(data){
          currentPage = data.page;
          return JSON.stringify(data);
        }
      }"
      class="display"
      width="100%"
    >
      <thead>
        <tr>
          <th>ID</th>
          <th>DESCRIPTION</th>
          <th>HOME</th>
          <th>REDE</th>
        </tr>
      </thead>
      <tfoot>
        <tr>
          <th>ID</th>
          <th>DESCRIPTION</th>
          <th>HOME</th>
          <th>REDE</th>
        </tr>
      </tfoot>
    </DataTable>
    <div>
      <button @click="previousPage" :disabled="currentPage === 0">Previous</button>
      <button @click="nextPage" :disabled="currentPage === totalPages - 1">Next</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import DataTable from 'datatables.net-vue3';
import DataTablesCore from 'datatables.net';
import 'datatables.net-select';
import 'datatables.net-responsive';

DataTable.use(DataTablesCore);

const columns = [
  { data: 'ID' },
  { data: 'DESCRIPTION' },
  { data: 'HOME' },
  { data: 'REDE' },
];

let currentPage = ref(0);
let totalPages = ref(1);

const apiUrl = `data3.json?page=${currentPage.value}`;

function fetchData(data) {
  data.page = currentPage.value;
  console.log(data.page);
}

async function fetchTotalPages() {
  // Simule uma requisição para obter o número total de páginas
  totalPages.value = await getTotalPages();
}

async function getTotalPages() {
  // Simule uma requisição para obter o número total de páginas
  // Aqui você pode chamar sua API ou calcular o número total de páginas com base nos dados
  return 10; // Suponha que existam 10 páginas no total
}

fetchTotalPages();

function nextPage() {
  if (currentPage.value < totalPages.value - 1) {
    currentPage.value++;
    apiUrl.value = `data3.json?page=${currentPage.value}`;
  }
}

function previousPage() {
  if (currentPage.value > 0) {
    currentPage.value--;
    apiUrl.value = `data3.json?page=${currentPage.value}`;
  }
}
</script>

<style>
@import 'datatables.net-dt';
</style>