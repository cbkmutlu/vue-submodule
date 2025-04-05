<template>
   <v-data-table
      v-bind="{ ...$attrs }"
      v-bind:class="{
         'v-table--sticky-header': props.stickyHeader,
         'v-table--sticky-footer': props.stickyFooter,
         'v-table--accent-header': props.accentHeader
      }"
      v-bind:disable-sort="disableSort"
      v-bind:items="filterItems"
      v-bind:items-per-page-options="[10, 25, 50, 100, -1]"
      v-bind:loading="!!props.loading ? 'primary' : false"
      density="compact"
      hover
      items-per-page="10"
      return-object
      @update:page="expandedItems = []">
      <template
         v-if="props.loading"
         v-slot:loading>
         <v-skeleton-loader type="table-row-divider, list-item-three-line, list-item-two-line, list-item-two-line" />
      </template>

      <template v-slot:top>
         <v-toolbar
            v-bind:color="props.color"
            class="rounded-t-sm"
            density="compact">
            <template v-slot:prepend>
               <v-avatar v-bind:color="props.color">
                  <v-icon>{{ props.titleIcon }}</v-icon>
               </v-avatar>
            </template>

            <v-toolbar-title class="text-lg">
               <slot name="title">{{ props.titleText }}</slot>
            </v-toolbar-title>

            <v-toolbar-items>
               <slot name="top">
                  <div class="m-1 flex items-center gap-2">
                     <slot name="top.prepend" />

                     <v-text-field
                        v-model="filterSearch"
                        append-inner-icon="$search"
                        hide-details
                        min-width="300"
                        @click:clear="filterDebounce = ''"
                        @input="filterInput($event)" />

                     <slot name="top.append" />
                  </div>
               </slot>
            </v-toolbar-items>
         </v-toolbar>
      </template>

      <template v-slot:headers="{ columns, isSorted, getSortIcon, toggleSort, someSelected, allSelected, selectAll }">
         <TableHeader
            v-if="!props.loading"
            v-bind="{ columns, isSorted, getSortIcon, toggleSort, someSelected, allSelected, selectAll, disableSort }" />
      </template>

      <template
         v-if="!props.loading"
         v-slot:body="{ internalItems, isSelected, toggleSelect, columns }">
         <v-fade-transition
            name="table"
            group
            hide-on-leave>
            <template
               v-for="item in internalItems"
               :key="item.value.id">
               <tr
                  v-bind:class="{ 'v-data-table__tr--clickable': rowClick }"
                  v-ripple="!!appConfig.default.ripple && !!rowClick"
                  class="v-data-table__tr"
                  @click="() => props.rowClick && props.rowClick(item.value)"
                  @click.ctrl.stop="(!itemExpand || item.value[itemExpand]) && toggleExpand(item)">
                  <template v-for="(column, index) in Object.keys(item.columns)">
                     <td
                        v-if="column === 'data-table-select'"
                        class="v-data-table__td v-data-table-column--no-padding v-data-table-column--align-start v-data-table__td--select-row">
                        <v-checkbox-btn
                           v-bind:disabled="!item.selectable"
                           v-bind:model-value="isSelected(item)"
                           v-ripple.stop
                           class="text-base"
                           @update:model-value="toggleSelect(item)" />
                     </td>

                     <td
                        v-else-if="column === 'data-table-expand'"
                        class="v-data-table__td v-data-table-column--no-padding v-data-table-column--align-start v-data-table__td--expanded-row">
                        <div class="flex justify-end">
                           <v-btn
                              v-bind:disabled="!!itemExpand && !item.value[itemExpand]"
                              v-ripple.stop
                              icon
                              @click.stop="(!itemExpand || item.value[itemExpand]) && toggleExpand(item)">
                              <ToggleIcon
                                 v-bind:icon="['$collapse', '$expand']"
                                 v-bind:toggle="isExpanded(item)"
                                 tabindex="-1" />
                           </v-btn>

                           <slot name="action" />
                        </div>
                     </td>

                     <td
                        v-else
                        v-bind:class="`v-data-table-column--align-${columns[index].align || 'start'}`"
                        class="v-data-table__td">
                        <slot
                           v-bind:column="columns[index]"
                           v-bind:item="item.value"
                           v-bind:name="`item.${column}`"
                           v-bind:value="item.columns[column]">
                           {{ item.columns[column] }}
                        </slot>
                     </td>
                  </template>
               </tr>

               <tr
                  v-if="isExpanded(item)"
                  v-bind:key="`expand-${item.value.id}`"
                  class="v-data-table__tr--expand v-data-table__tr">
                  <td v-bind:colspan="columns.length">
                     <slot
                        v-bind:item="item.value"
                        name="expand" />
                  </td>
               </tr>
            </template>
         </v-fade-transition>
      </template>
   </v-data-table>
</template>

<script lang="ts" setup>
import { TDataTable } from "@/utils/types";
import ToggleIcon from "../Input/ToggleIcon.vue";
import TableHeader from "./TableHeader.vue";

type TProps = {
   items?: any[];
   stickyHeader?: boolean;
   stickyFooter?: boolean;
   accentHeader?: boolean;
   disableSort?: boolean;
   rowClick?: (item: any) => any;
   titleText?: string;
   titleIcon?: string;
   itemExpand?: string;
   multiExpand?: boolean;
   loading?: boolean;
};

const props = withDefaults(defineProps<TDataTable & TProps>(), {
   stickyHeader: true,
   stickyFooter: true,
   accentHeader: true,
   disableSort: false,
   titleIcon: "$ratingEmpty",
   itemExpand: ""
});

const filterSearch = ref("");
const filterDebounce = ref("");
const filterInput = timerDebounce(async ($event) => {
   filterDebounce.value = $event.target.value;
});
const filterItems = computed(() => {
   return props.items?.filter((item: any) => {
      let value = filterDebounce.value.toString().toLowerCase();
      return !(value && !inputFilter(lowerCase(Object.values(item).join(" ").toString()), value));
   });
});

const expandedItems: any = ref([]);
const isExpanded = (item: any) => expandedItems.value.includes(item);
const toggleExpand = (item: any, multiple: boolean = props.multiExpand) => {
   if (multiple) {
      if (isExpanded(item)) {
         expandedItems.value.splice(expandedItems.value.indexOf(item), 1);
      } else {
         expandedItems.value.push(item);
      }
   } else {
      expandedItems.value = isExpanded(item) ? [] : [item];
   }
};
</script>
