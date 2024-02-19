<script setup lang="ts">
import DataTable from 'datatables.net-vue3';
import DataTablesCore from 'datatables.net';
import 'datatables.net-select';
import 'datatables.net-responsive';
import { ref, onMounted } from 'vue';
import $ from 'jquery';

DataTable.use($);

const apiUrl = `http://localhost:8080/company`;

let ajaxConfig = {
  url: apiUrl,
  dataSrc: 'content'
};
let totalPages: number | null = null;

onMounted(() => {
  $.ajax({
    url: apiUrl,
    method: 'GET',
    success: function (data) {
      totalPages = data.totalPages;
      console.log(totalPages);
      const table = $('#example').DataTable(options.value);

      table.on('draw.dt', function () {
        const pageInfo = table.page.info();
        console.log("Número total de registros: ", pageInfo.recordsTotal);
        console.log("Número total de páginas: ", pageInfo.pages);
        console.log("Número da página atual: ", pageInfo.page);
        console.log("Índice do primeiro registro nesta página: ", pageInfo.start);
        console.log("Índice do último registro nesta página: ", pageInfo.end);
        console.log("Número de registros nesta página: ", pageInfo.length);
      });
    },
    error: function (error) {
      console.error('Erro ao obter o total de páginas:', error);
    }
  });

});


const columns = [
  { data: 'ID' },
  { data: 'DESCRIPTION' },
  { data: 'HOME' },
  { data: 'REDE' },
];


interface Config {
  lengthMenu: [number[], (number | string)[]];
  language: {
    searchPlaceholder: string;
    search: string;
    zeroRecords: string;
    emptyTable: string;
    lengthMenu: string;
    paginate: {
      previous: string;
      next: string;
    };
  };
  select: boolean;
  responsive: boolean;
  paging: boolean;
  pagingType: string;
}

const options = ref<Config>({
  lengthMenu: [
    [10, 20, 30, 40, 50, 100, -1],
    [10, 20, 30, 40, 50, 100, "All"],
  ],
  language: {
    searchPlaceholder: "Filter users...",
    search: "",
    zeroRecords: "No users found",
    emptyTable: "No users found",
    lengthMenu: "Rows per page _MENU_",
    paginate: {
      previous: "Prev",
      next: "Next"
    }
  },
  select: true,
  responsive: true,
  paging: true,
  pagingType: 'full_numbers',
  
});




</script>

<template>
  <div>
    <h1>Ajax data source table</h1>

    <DataTable :columns="columns" :options="options" id="example" :ajax="ajaxConfig" class="display" width="100%">
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
  </div>
</template>

<style>
@import 'datatables.net-dt';
</style>
