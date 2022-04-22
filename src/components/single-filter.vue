<template>
  <div class="col-custom" v-for="filt in filters" :key="filt.label">
    <div spread class="single-filter">
      <div class="single-filter__header flex justify-between items-center">
        <span class="single-filter__label q-mb-xs flex">{{ filt }}</span>

        <div>
          <q-checkbox v-model="val" label="Ввести вручную" size="xs" />
          <q-btn size="xs" flat round icon="close" @click="removeFilter(filt)">
          </q-btn>
        </div>
      </div>

      <q-select dense outlined v-model="model" />
    </div>
  </div>
</template>

<script>
export default {
  props: [],

  setup(props) {},
};
</script>

<style lang="scss"></style>
