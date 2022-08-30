<template>
  <component :is="icon" />
</template>

<script lang="ts">
import * as icons from "lucide-vue-next";
import { computed } from "vue";

// eslint-disable-next-line @typescript-eslint/no-explicit-any
const fixedIcons = icons as any;

export default {
  props: {
    name: {
      type: String,
      required: true,
    },
  },
  setup(props: { name: string | number }) {
    const icon = computed(() => fixedIcons[props.name]);

    return { icon };
  },
};
</script>
