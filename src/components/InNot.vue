<template>
  <q-select
    dense
    outlined
    v-model="isNotModel"
    :options="options"
    @update:model-value="
      (value) => {
        $emit('changeIsNotModel', value, id);
      }
    "
  />
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "InNot",
  props: ["idx"],

  setup(props) {
    let isNotModel = ref("in");
    let id = ref(props.idx);

    return {
      isNotModel,
      id,
      options: ["in", "NotIn", ">", "<"],
    };
  },
});
</script>

<style lang="scss"></style>
