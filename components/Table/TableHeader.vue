<template>
   <tr>
      <template
         v-for="column in columns"
         :key="column.key">
         <th
            v-if="column.key === 'data-table-select'"
            v-bind:style="{ width: '52px' }"
            class="v-data-table__td v-data-table-column--no-padding v-data-table-column--align-start v-data-table__th">
            <v-checkbox-btn
               v-bind:indeterminate="someSelected && !allSelected"
               v-bind:model-value="allSelected"
               class="text-base"
               @update:model-value="selectAll(!allSelected)" />
         </th>

         <th
            v-else-if="column.key === 'data-table-expand'"
            v-bind:style="{ width: '52px' }"
            class="v-data-table__td v-data-table-column--no-padding v-data-table-column--align-start v-data-table__th"></th>

         <th
            v-else
            v-bind:class="[`v-data-table-column--align-${column.align || 'start'}`, { 'v-data-table__th--sortable': !disableSort && column.sortable }]"
            v-bind:style="{ width: column.width ? column.width + 'px' : '100%' }"
            class="v-data-table__td v-data-table__th">
            <div
               class="v-data-table-header__content"
               @click="!disableSort && column.sortable && toggleSort(column)">
               <span
                  v-bind:class="{ 'opacity-100': isSorted(column) }"
                  class="me-2 opacity-60">
                  {{ column.title }}
               </span>

               <v-icon
                  v-if="!disableSort && column.sortable"
                  v-bind:class="{ 'opacity-100': isSorted(column) }"
                  class="v-data-table-header__sort-icon"
                  :icon="getSortIcon(column)"></v-icon>

               <!-- <v-icon
                  v-if="column.removable"
                  color="medium-emphasis"
                  icon="$close"
                  @click="remove(column.key)"></v-icon> -->
            </div>
         </th>
      </template>
   </tr>
   <!-- <tr>
      <th :colspan="columns.length"></th>
   </tr> -->
</template>

<script setup>
defineProps(["columns", "isSorted", "getSortIcon", "toggleSort", "someSelected", "allSelected", "selectAll", "disableSort"]);
</script>
