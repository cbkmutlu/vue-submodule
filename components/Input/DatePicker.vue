<template>
   <v-menu
      v-model="dateMenu"
      v-bind:close-on-content-click="false"
      v-bind:offset="props.hideDetails ? [0, 0] : [-22, 0]"
      class="[&_.v-overlay\_\_content]:!min-w-min"
      eager
      transition="fade-transition">
      <template v-slot:activator="{ props: activatorProps }">
         <v-text-field
            v-bind="{ ...activatorProps, ...$attrs }"
            v-bind:active="dateMenu"
            v-bind:hide-details="props.hideDetails"
            v-bind:model-value="dateDisplay"
            v-bind:readonly="props.readonly"
            v-bind:title="props.title"
            tabindex="1"
            @click:clear="dateMenu = !!props.openOnClear"
            @update:model-value="model = null!" />
      </template>

      <v-card class="overflow-x-hidden">
         <v-toolbar
            v-if="props.title !== ''"
            color="primary"
            density="compact">
            <v-toolbar-title class="text-base">
               <slot
                  v-bind:model
                  name="title">
                  <template v-if="props.title === null">
                     {{ formatDate(model, appConfig.format.picker as unknown as Intl.DateTimeFormatOptions) || props.empty || t("app.noDate") }}
                  </template>

                  <template v-else>
                     {{ props.title }}
                  </template>
               </slot>
            </v-toolbar-title>
         </v-toolbar>

         <v-date-picker
            v-model="model"
            v-bind="{ ...$attrs }"
            v-bind:first-day-of-week="props.firstDayOfWeek"
            v-bind:show-adjacent-months="props.showAdjacentMonths"
            hide-header
            width="300"
            @update:model-value="dateHandler($event as unknown as Date)" />

         <v-card-actions v-if="props.actions">
            <v-btn
               color="error"
               variant="tonal"
               @click="dateMenu = false">
               {{ t("app.cancel") }}
            </v-btn>

            <v-spacer></v-spacer>

            <v-btn
               variant="plain"
               @click="dateHandler(new Date(), 0)">
               {{ t("app.today") }}
            </v-btn>

            <v-btn
               variant="plain"
               @click="dateHandler(new Date(), 1)">
               {{ t("app.tomorrow") }}
            </v-btn>
         </v-card-actions>
      </v-card>
   </v-menu>
</template>

<script lang="ts" setup>
import { TDateField } from "@/utils/types";

type TProps = {
   title?: string;
   readonly?: boolean;
   actions?: boolean;
   hideDetails?: boolean;
   empty?: string;
   firstDayOfWeek?: number;
   showAdjacentMonths?: boolean;
   closeOnPickerClick?: boolean;
   openOnClear?: boolean;
   color?: string;
};

const props = withDefaults(defineProps<TDateField & TProps>(), {
   title: null as unknown as string,
   readonly: true,
   actions: true,
   hideDetails: false,
   empty: "",
   closeOnPickerClick: true,
   showAdjacentMonths: true,
   firstDayOfWeek: 1,
   openOnClear: true
});

defineOptions({ inheritAttrs: false });

const model = defineModel({ type: [Date, String], default: undefined });
const { t } = useI18n();

const dateMenu = ref(false);
const dateDisplay = computed(() => formatDate(model.value));

const dateHandler = (value: Date, day?: number) => {
   if (typeof day === "number") {
      value.setDate(value.getDate() + day);
   }

   formatMS(value);
   model.value = value as unknown as string;
   dateMenu.value = !props.closeOnPickerClick;
};

// const dateDisplay = ref();
// watchEffect(() => (dateDisplay.value = formatDate(date.value)));
</script>
