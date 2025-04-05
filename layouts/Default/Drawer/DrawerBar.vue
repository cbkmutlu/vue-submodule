<template>
   <v-navigation-drawer
      v-bind:class="{ '!w-56': expand }"
      class="select-none not-dark:bg-indigo-900 not-dark:text-white"
      rail>
      <div class="flex-center my-2 flex w-14">
         <v-btn
            v-bind="{ ...(expand && { variant: 'tonal' }) }"
            icon
            @click="expand = !expand">
            <ToggleIcon
               v-bind:icon="['$first', '$last']"
               v-bind:toggle="expand"
               tabindex="-1" />
         </v-btn>
      </div>

      <v-list
         class="flex flex-1 flex-col"
         density="compact">
         <v-list-item link>
            <template v-slot:prepend>
               <v-icon
                  icon="$branch"
                  size="default" />
            </template>

            <template v-slot:title>Bayi Yönetimi</template>
         </v-list-item>

         <div class="flex-1"></div>

         <v-list-item
            link
            tile>
            <template v-slot:prepend>
               <v-icon
                  icon="$settings"
                  size="default" />
            </template>

            <template v-slot:title>Ayarlar</template>
         </v-list-item>
      </v-list>
   </v-navigation-drawer>
</template>

<script lang="ts" setup>
import ToggleIcon from "@/assets/components/Input/ToggleIcon.vue";

const expand = ref(false);
</script>
