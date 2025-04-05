<template>
   <v-menu offset="3, 0">
      <template v-slot:activator="{ props }">
         <v-btn
            v-bind="props"
            icon="$dots" />
      </template>

      <v-list
         v-bind:slim="false"
         class="select-none"
         density="compact">
         <v-list-item
            append-icon="$accountProfile"
            link>
            <v-list-item-title>{{ t("account.profile") }}</v-list-item-title>
         </v-list-item>

         <v-list-item
            append-icon="$accountLogout"
            link
            @click="logoutHandler">
            <v-list-item-title>{{ t("account.logout") }}</v-list-item-title>
         </v-list-item>
      </v-list>
   </v-menu>
</template>

<script lang="ts" setup>
const { t } = useI18n();
const authStore = useAuthStore();

const logoutHandler = () => {
   authStore.userLogout();
};
</script>
