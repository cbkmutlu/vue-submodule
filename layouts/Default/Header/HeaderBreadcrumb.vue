<template>
   <v-breadcrumbs
      class="text-xs"
      density="compact">
      <template
         v-for="(item, index) in items"
         v-bind:key="item">
         <v-breadcrumbs-item
            v-bind:class="{ 'font-semibold': index === items.length - 1 }"
            v-bind:disabled="index === items.length - 1"
            v-bind:exact="appConfig.router.exact"
            v-bind:to="item.path">
            {{ t(item.breadcrumb!) }}
         </v-breadcrumbs-item>

         <v-icon
            v-if="index < items.length - 1"
            icon="$chevronRight" />
      </template>
   </v-breadcrumbs>
</template>

<script lang="ts" setup>
const { t } = useI18n();
const route = useRoute();

const items = computed(() => {
   return route.matched.filter((item) => item.meta.breadcrumb).map((item) => ({ breadcrumb: item.meta.breadcrumb, path: item.path }));
});
</script>
