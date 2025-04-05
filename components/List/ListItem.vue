<template>
   <v-list-subheader v-if="props.item.itemType === 'subheader'">
      {{ props.item.itemTitle }}
   </v-list-subheader>

   <v-divider
      v-else-if="props.item.itemType === 'divider'"
      class="mb-1" />

   <v-list-item
      v-else
      v-bind="props.item.itemProps">
      <template v-slot:append="{ isActive }">
         <v-icon
            v-if="props.item.children"
            v-bind:class="{ 'rotate-180': isActive }"
            class="transition duration-200"
            icon="$dropdown"
            size="x-small" />
      </template>

      <v-list-item-title class="flex items-center">
         <v-icon
            v-if="props.item.itemProps?.icon"
            v-bind:icon="props.item.itemProps?.icon"
            class="opacity-[var(--v-medium-emphasis-opacity)]" />

         <div>{{ props.item.itemTitle }}</div>
      </v-list-item-title>
   </v-list-item>
</template>

<script lang="ts" setup>
const props = defineProps({
   item: {
      type: Object as () => TList,
      required: true
   }
});
</script>
