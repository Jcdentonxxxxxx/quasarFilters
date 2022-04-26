<template>
  <q-select
    dense
    outlined
    v-model="isNotModel"
    :options="options"
    @update:model-value="
      (value) => {
        $emit('changeIsNotModel', value);
      }
    "
  />
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "InNot",

  setup() {
    let isNotModel = ref("in");

    return {
      isNotModel,

      options: ["in", "NotIn", ">", "<"],
    };
  },
});
</script>

<style lang="scss"></style>
