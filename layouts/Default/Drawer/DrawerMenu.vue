<template>
   <v-navigation-drawer
      class="select-none not-dark:bg-blue-900 not-dark:text-white"
      width="224">
      <v-list density="compact">
         <template
            v-for="item in appMenu"
            v-bind:key="item">
            <ListGroup
               v-if="item.children"
               v-bind:item="item" />

            <ListItem
               v-else
               v-bind:item="item" />
         </template>
      </v-list>
   </v-navigation-drawer>
</template>

<script lang="ts" setup>
import ListGroup from "@/assets/components/List/ListGroup.vue";
import ListItem from "@/assets/components/List/ListItem.vue";
</script>
