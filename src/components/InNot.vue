<template>
  <q-select
    dense
    outlined
    v-model="isNotModelCurrent"
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
let optionsStr = ["in", "NotIn"];
let optionInt = [">", "<"];

export default defineComponent({
  name: "InNot",
  props: ["idx", "typeIsNot", "pickIsNot"],

  setup(props) {
    let isNotModel = ref("in");
    let id = ref(props.idx);
    let isNotModelCurrent = ref(props.pickIsNot);

    return {
      isNotModel,
      id,
      isNotModelCurrent,
      options: props.typeIsNot ? optionsStr : optionInt,
    };
  },
});
</script>

<style lang="scss"></style>
