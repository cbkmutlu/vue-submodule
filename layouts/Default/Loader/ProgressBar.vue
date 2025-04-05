<template>
   <v-fade-transition>
      <v-progress-linear
         v-if="appStore.componentLoading"
         class="absolute top-0 z-[1005]"
         color="primary"
         height="3"
         indeterminate />
   </v-fade-transition>
</template>

<script lang="ts" setup>
const appStore = useAppStore();
</script>
