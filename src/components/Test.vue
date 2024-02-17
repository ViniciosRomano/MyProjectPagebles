<script setup lang="ts">
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

</script>

<template>
  <div>
    <h1>Ajax data source table</h1>

    <DataTable 
      :columns="columns"
      id="example"
      :ajax="{
        url: '/data3.json',
        dataSrc: 'content',
        data: function(data) {
          // Adicione os parâmetros de paginação aqui
          data.pageNumber = data.start / data.length + 1; // Calcula o número da página
          data.pageSize = data.length; // Tamanho da página
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
  </div>
</template>

<style>
@import 'datatables.net-dt';
</style>