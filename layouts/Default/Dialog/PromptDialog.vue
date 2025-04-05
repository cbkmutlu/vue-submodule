<template>
   <v-dialog
      v-model="promptStore.dialog.show"
      v-bind:max-width="promptStore.dialog.width"
      transition="fade-transition"
      @after-leave="promptStore.reset()"
      @keydown.esc="promptStore.cancel()">
      <v-card v-bind:loading="promptStore.dialog.loading">
         <v-form
            v-model="promptStore.dialog.validate"
            @submit.prevent="promptStore.dialog.validate && promptStore.accept()">
            <v-toolbar v-bind:color="promptStore.dialog.titleColor">
               <v-toolbar-title class="text-base">
                  {{ promptStore.dialog.title || t("app.promptTitle") }}
               </v-toolbar-title>

               <template v-slot:append>
                  <v-btn
                     icon="$close"
                     variant="text"
                     tabindex="-1"
                     @click="promptStore.cancel()" />
               </template>
            </v-toolbar>

            <v-card-text class="text-sm">
               <v-text-field
                  v-model.trim="promptStore.dialog.message"
                  v-bind:autofocus="promptStore.dialog.focus"
                  v-bind:label="promptStore.dialog.label"
                  v-bind:readonly="promptStore.dialog.request"
                  v-bind:rules="promptStore.dialog.rules"
                  tabindex="1"
                  @input="promptStore.dialog.callback && (promptStore.dialog.message = promptStore.dialog.callback(promptStore.dialog.message))" />
            </v-card-text>

            <v-divider></v-divider>

            <v-card-actions class="justify-end">
               <v-btn
                  v-bind:color="promptStore.dialog.cancelColor"
                  v-bind:variant="promptStore.dialog.cancelVariant"
                  tabindex="1"
                  @click="promptStore.cancel()">
                  {{ promptStore.dialog.cancelText || t("app.cancel") }}
               </v-btn>

               <v-btn
                  v-bind:color="promptStore.dialog.acceptColor"
                  v-bind:disabled="!promptStore.dialog.validate"
                  v-bind:loading="promptStore.dialog.request"
                  v-bind:variant="promptStore.dialog.acceptVariant || 'tonal'"
                  v-directive-focus
                  type="submit"
                  tabindex="1"
                  @click="promptStore.accept()">
                  {{ promptStore.dialog.acceptText || t("app.accept") }}
               </v-btn>
            </v-card-actions>
         </v-form>
      </v-card>
   </v-dialog>
</template>

<script lang="ts" setup>
const { t } = useI18n();
const promptStore = usePromptStore();
</script>
