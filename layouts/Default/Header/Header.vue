<template>
   <v-app-bar density="compact">
      <!-- <template v-slot:prepend>
         <v-app-bar-nav-icon
            density="comfortable"
            icon="$arrowLeft"
            @click="$router.back()"></v-app-bar-nav-icon>
      </template> -->

      <template v-slot:title>
         {{ title }}
      </template>

      <template v-slot:extension>
         <HeaderBreadcrumb />
      </template>

      <template
         v-slot:append
         class="flex gap-2">
         <div class="flex gap-2">
            <v-btn icon="$search" />
            <v-btn icon="$notification" />
            <v-btn
               icon
               @click="setUserTheme(theme, true)">
               <ToggleIcon
                  v-bind:icon="['$themeLight', '$themeDark']"
                  v-bind:toggle="theme.current.value.dark"
                  tabindex="-1" />
            </v-btn>
            <HeaderLocaleMenu />
            <HeaderMoreMenu />
         </div>
      </template>
   </v-app-bar>
</template>

<script lang="ts" setup>
import ToggleIcon from "@/assets/components/Input/ToggleIcon.vue";
import HeaderBreadcrumb from "./HeaderBreadcrumb.vue";
import HeaderLocaleMenu from "./HeaderLocaleMenu.vue";
import HeaderMoreMenu from "./HeaderMoreMenu.vue";

const theme = useTheme();
const route = useRoute();

const title = computed(() => {
   return route.matched
      .filter((item) => item.meta?.title)
      .map((item) => item.meta.title)
      .slice(-1)[0];
});
</script>
