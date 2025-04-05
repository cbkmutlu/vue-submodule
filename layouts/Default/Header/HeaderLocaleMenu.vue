<template>
   <v-menu offset="3, 0">
      <template v-slot:activator="{ props }">
         <v-btn
            v-bind="props"
            v-bind:loading="appStore.localeLoading"
            icon="$translate" />
      </template>

      <v-list
         v-bind:slim="false"
         class="select-none"
         density="compact">
         <v-list-item
            v-for="(locale, key) in appConfig.support.locale"
            @click="translateHandler(key)">
            <template v-slot:append>
               <v-icon v-html="locale.flag" />
            </template>

            <template v-slot:title>
               {{ locale.name }}
            </template>
         </v-list-item>
      </v-list>
   </v-menu>
</template>

<script lang="ts" setup>
const appStore = useAppStore();

const translateHandler = async (locale: string) => {
   appStore.setLocaleLoading(true);
   await loadLocales(locale).then(() => {
      setUserLocale(locale);
      appStore.setLocaleLoading(false);
   });
};
</script>
