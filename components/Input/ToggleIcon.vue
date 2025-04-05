<template>
   <v-fade-transition leave-absolute>
      <v-icon
         v-if="props.toggle"
         v-bind:icon="props.icon[0]" />

      <v-icon
         v-else
         v-bind:icon="props.icon[1]" />
   </v-fade-transition>
</template>

<script lang="ts" setup>
const props = defineProps({
   icon: {
      type: Array as () => string[],
      required: true
   },
   toggle: {
      type: Boolean,
      required: true
   }
});
</script>
