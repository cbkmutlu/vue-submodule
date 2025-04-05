<template>
   <v-app>
      <DrawerBar />
      <DrawerMenu />
      <Header />

      <v-main class="h-full transition-none">
         <ProgressBar />

         <div class="v-main__scroller h-full overflow-y-scroll scroll-smooth p-6 not-dark:bg-neutral-50">
            <router-view v-slot="{ Component }">
               <v-slide-x-transition leave-absolute>
                  <!-- <keep-alive> -->
                  <component v-bind:is="Component" />
                  <!-- </keep-alive> -->
               </v-slide-x-transition>
            </router-view>
         </div>
      </v-main>

      <ConfirmDialog />
      <PromptDialog />
   </v-app>
</template>

<script lang="ts" setup>
import ConfirmDialog from "./Dialog/ConfirmDialog.vue";
import PromptDialog from "./Dialog/PromptDialog.vue";
import DrawerBar from "./Drawer/DrawerBar.vue";
import DrawerMenu from "./Drawer/DrawerMenu.vue";
import Header from "./Header/Header.vue";
import ProgressBar from "./Loader/ProgressBar.vue";
</script>
