<template>
   <v-dialog
      v-model="confirmStore.dialog.show"
      v-bind:max-width="confirmStore.dialog.width"
      transition="fade-transition"
      @after-leave="confirmStore.reset()"
      @keydown.esc="confirmStore.cancel()">
      <v-card v-bind:loading="confirmStore.dialog.loading">
         <v-toolbar v-bind:color="confirmStore.dialog.titleColor">
            <v-toolbar-title class="text-base">
               {{ confirmStore.dialog.title || t("app.confirmTitle") }}
            </v-toolbar-title>

            <template v-slot:append>
               <v-btn
                  icon="$close"
                  variant="text"
                  tabindex="-1"
                  @click="confirmStore.cancel()" />
            </template>
         </v-toolbar>

         <v-card-text
            v-if="confirmStore.dialog.message"
            class="text-sm">
            {{ confirmStore.dialog.message }}
         </v-card-text>

         <v-divider></v-divider>

         <v-card-actions>
            <v-btn
               v-bind:color="confirmStore.dialog.cancelColor"
               v-bind:variant="confirmStore.dialog.cancelVariant"
               tabindex="1"
               @click="confirmStore.cancel()">
               {{ confirmStore.dialog.cancelText || t("app.cancel") }}
            </v-btn>

            <v-btn
               v-bind:color="confirmStore.dialog.acceptColor"
               v-bind:loading="confirmStore.dialog.request"
               v-bind:variant="confirmStore.dialog.acceptVariant || 'tonal'"
               v-directive-focus
               type="submit"
               tabindex="1"
               @click="confirmStore.accept()">
               {{ confirmStore.dialog.acceptText || t("app.accept") }}
            </v-btn>
         </v-card-actions>
      </v-card>
   </v-dialog>
</template>

<script lang="ts" setup>
const { t } = useI18n();
const confirmStore = useConfirmStore();
</script>
