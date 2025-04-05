<template>
   <v-select
      v-model="model"
      v-bind="{ ...$attrs }"
      :items="filterItems"
      :menu-props="{ width: 300, maxHeight: 320 }"
      :multiple="props.multiple"
      :open-on-clear="props.openOnClear"
      transition="fade-transition"
      tabindex="1"
      @click:clear="clearFilter">
      <template
         v-if="props.multiple || props.search"
         v-slot:prepend-item>
         <v-list-item
            v-on="!props.search ? { click: selectAll(!allSelected) } : {}"
            link>
            <template
               v-if="props.multiple"
               v-slot:prepend>
               <v-checkbox-btn
                  v-bind:indeterminate="someSelected && !allSelected"
                  v-bind:model-value="allSelected"
                  v-bind:ripple="false"
                  class="text-base"
                  @update:model-value="selectAll(!allSelected)" />
            </template>

            <v-list-item-title>
               <slot name="header">
                  <template v-if="props.search">
                     <v-text-field
                        v-model="filterSearch"
                        hide-details
                        @click:clear="filterDebounce = ''"
                        @input="filterInput($event)"
                        tabindex="-1"
                        @keydown="handleKeydown($event)">
                        <template v-slot:append-inner>
                           <v-icon>$search</v-icon>
                        </template>
                     </v-text-field>
                  </template>
                  <template v-else-if="props.multiple">
                     {{ t("app.selectAll") }}
                  </template>
               </slot>
            </v-list-item-title>
         </v-list-item>

         <v-divider class="my-2"></v-divider>
      </template>

      <template v-slot:item="{ item, props: itemProps }">
         <v-list-item v-bind="itemProps">
            <template
               v-if="props.multiple"
               v-slot:prepend="{ isSelected }">
               <v-checkbox-btn
                  v-bind:model-value="isSelected"
                  v-bind:ripple="false"
                  class="text-base" />
            </template>

            <template v-slot:title>
               <slot
                  v-bind:item="item"
                  name="item">
                  {{ item.title }}
               </slot>
            </template>
         </v-list-item>
      </template>

      <template v-slot:selection="{ item, index }">
         <template v-if="index === 0 && model.length > props.count && props.count > 0">
            <v-chip
               class="mr-1"
               color="primary"
               density="compact"
               size="small"
               variant="tonal">
               {{ model.length }}
            </v-chip>
         </template>

         <template v-if="index < props.count || props.count === 0">
            <span class="v-select__selection-text">
               <span>
                  {{ item.title }}
               </span>
               <template v-if="index < model.length - 1 && (index < props.count - 1 || props.count === 0)">
                  <span class="v-select__selection-comma">,</span>
               </template>
            </span>
         </template>
      </template>
   </v-select>
</template>

<script lang="ts" setup>
import { TMultiSelect } from "@/utils/types";

type TProps = {
   items?: any[];
   count?: number;
   search?: boolean;
   multiple?: boolean;
   openOnClear?: boolean;
   menuProps?: any;
};

const props = withDefaults(defineProps<TMultiSelect & TProps>(), {
   count: 2,
   search: true,
   multiple: false,
   openOnClear: true
});
const model = defineModel({ type: [Array, Object, Number, null], default: {} });
const { t } = useI18n();

const allSelected = computed(() => model.value.length === filterItems.value?.length);
const someSelected = computed(() => model.value.length > 0);

const selectAll = (value: boolean) => {
   model.value = value ? filterItems.value?.slice() : [];
};

const clearFilter = () => {
   filterDebounce.value = "";
   filterSearch.value = "";
};

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

const handleKeydown = (event: KeyboardEvent) => event.key === "Escape" || event.stopPropagation();
</script>
