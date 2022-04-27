<template>
  <q-select
    dense
    outlined
    input-debounce="0"
    :options="child"
    @filter="filterFn"
  />
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "SingleFilter",
  props: ["filterOption", "parentFilterOptions", "isNotModels"],

  setup(props) {
    let child = ref(props.filterOption);
    let parent = ref(props.parentFilterOptions);

    return {
      child,
      parent,

      filterFn(val, update) {
        if (val === "") {
          update(() => {
            child.value = parent.value;
            // here you have access to "ref" which
            // is the Vue reference of the QSelect
          });
          return;
        }
        console.log(props.isNotModels);

        if (props.isNotModels === "in") {
          update(() => {
            const needle = val.toLowerCase();
            child.value = parent.value.filter((v) => {
              return v.toLowerCase().indexOf(needle) > -1;
            });
          });
        }

        if (props.isNotModels === "NotIn") {
          update(() => {
            const needle = val.toLowerCase();
            child.value = parent.value.filter((v) => {
              return v.toLowerCase().indexOf(needle) === -1;
            });
          });
        }
      },
    };
  },
});
</script>

<style lang="scss"></style>
