<template>
  <q-page class="main">
    <div class="row-custom q-pa-xl">
      <div class="col-custom" v-for="filt in filters" :key="filt.label">
        <div spread class="single-filter">
          <div class="single-filter__header flex justify-between items-center">
            <span class="single-filter__label q-mb-xs flex">{{
              filt.value
            }}</span>

            <div>
              <q-checkbox
                v-model="filt.manual"
                label="Ввести вручную"
                size="xs"
              />
              <q-btn
                size="xs"
                flat
                round
                icon="close"
                @click="removeFilter(filt)"
              >
              </q-btn>
            </div>
          </div>

          <!-- <q-select
            dense
            outlined
            v-model="filt.model"
            :options="filt.options"
            :use-input="filt.manual"
            :hide-dropdown-icon="filt.manual"
            input-debounce="0"
            @filter="filterFn"
          /> -->
          <div class="row justify-between no-wrap">
            <div class="col-9 q-mr-xs">
              <SingleFilter
                v-model="filt.model"
                :use-input="filt.manual"
                :hide-dropdown-icon="filt.manual"
                :filterOption="filt.options"
                :parentFilterOptions="filt.parentOptions"
                :isNotModels="filt.isNotModelllllll"
              ></SingleFilter>
            </div>
            <div class="col-3 innot">
              <InNot
                :idx="filt.id"
                @change-is-not-model="(n, idx) => changeIsNotModels(n, idx)"
              ></InNot>
            </div>
          </div>
        </div>
      </div>
      <div class="col-custom" v-if="options.length > 0">
        <div class="filters">
          <span class="filters__label flex">Выбрать фильтр</span>
          <q-select
            dense
            outlined
            v-model="text"
            :options="options"
            @update:model-value="(val) => addFilter(val)"
            :hide-dropdown-icon="false"
          />
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref, reactive } from "vue";
import SingleFilter from "components/SingleFilter.vue";

import InNot from "components/InNot.vue";

let flowsOptions = [
  "flow",
  "stream",
  "streamers",
  "current",
  "flood",
  "torrent",
  "tide",
];

let landsOptions = [
  "land_id_sustav",
  "land_uz_poten",
  "land_ua_sustav_rul",
  "land_kz_zren_box",
  "land_ru_zren_rul",
  "land_po_vinir_box",
  "land_ro_sustav_new_chest",
];

let preLandsOptions = [
  "v2000_id_sustav",
  "v2002_uz_poten",
  "v2010_ua_sustav_rul",
  "v2011_kz_zren_box",
  "v1858_ru_zren_rul",
  "v2114_po_vinir_box",
  "v2200_ro_sustav_new_chest",
];

export default defineComponent({
  name: "IndexPage",

  components: {
    SingleFilter,
    InNot,
  },

  setup() {
    const flows = ref(flowsOptions);
    const lands = ref(landsOptions);
    const preLands = ref(preLandsOptions);
    let textFlows = ref(null);
    let textLands = ref(null);
    let preLandsText = ref(null);
    let isNotModelllllll = ref("in");

    const stringOptions = [
      {
        id: 1,
        label: "Потоки",
        value: "Потоки",
        manual: false,
        options: flows,
        parentOptions: flowsOptions,
        model: textFlows,
        isNotModelllllll: "in",
      },
      {
        id: 2,
        label: "Лендинги",
        value: "Лендинги",
        manual: false,
        options: lands,
        parentOptions: landsOptions,
        model: textLands,
        isNotModelllllll: "in",
      },
      {
        id: 3,
        label: "Прелендинги",
        value: "Прелендинги",
        manual: false,
        options: preLands,
        parentOptions: preLandsOptions,
        model: preLandsText,
        isNotModelllllll: "in",
      },
      {
        id: 4,
        label: "Выплаты",
        value: "Выплаты",
        options: false,
        isNotModelllllll: "in",
      },
      {
        id: 5,
        label: "Поступления",
        value: "Поступления",
        options: false,
        isNotModelllllll: "in",
      },
    ];

    return {
      text: ref(null),

      val: ref(true),

      filters: reactive([]),
      textLands,
      textFlows,
      flows,
      lands,
      preLands,
      isNotModelllllll,

      options: reactive(stringOptions),

      changeIsNotModels(val, idx) {
        // this.isNotModelllllll = val;
        let obj = this.filters.find((item) => item.id == idx);

        obj.isNotModelllllll = val;
        console.log(obj.isNotModelllllll);
      },

      addFilter(val) {
        this.filters.push(val);

        // let idx = stringOptions.indexOf(val);
        let idx = stringOptions.findIndex(function (item) {
          return item.id === val.id;
        });

        stringOptions.splice(idx, 1);
        this.text = "";
      },

      removeFilter(val) {
        let idx = this.filters.indexOf(val);
        this.filters.splice(idx, 1);
        this.options.push(val);
      },

      filterFn(val, update) {
        if (val === "") {
          update(() => {
            flows.value = flowsOptions;

            // here you have access to "ref" which
            // is the Vue reference of the QSelect
          });
          return;
        }

        update(() => {
          const needle = val.toLowerCase();
          flows.value = flowsOptions.filter((v) => {
            return v.toLowerCase().indexOf(needle) > -1;
          });
        });
      },

      // options: [
      //   {
      //     label: "Потоки",
      //     value: "Потоки",
      //     type: "duble",
      //   },
      //   {
      //     label: "Лендинги",
      //     value: "Лендинги",
      //     type: "duble",
      //   },
      //   {
      //     label: "Прелендинги",
      //     value: "Прелендинги",
      //     type: "duble",
      //   },
      //   {
      //     label: "Выплаты",
      //     value: "Выплаты",
      //     type: "single",
      //   },
      //   {
      //     label: "Поступления",
      //     value: "Поступления",
      //     type: "single",
      //   },
      // ],
    };
  },
});
</script>

<style lang="scss">
.main {
  max-width: 1200px;
  margin: 0 auto;
}
.row-custom {
  margin: 0 -10px;
  padding-top: 50px;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  flex-wrap: wrap;
}
.col-custom {
  padding: 5px;
  flex: 1 1 33.33333%;
  max-width: 33.33333%;
}
.filters {
  border: 2px solid #880e4f;
  min-width: 300px;
  padding: 10px;

  &__label {
    font-weight: 700;
    min-height: 30px;
    display: block;
  }

  label {
    overflow: hidden;
  }

  .q-field--outlined .q-field__control {
    padding: 0 0 0 12px;

    .q-field__append {
      padding: 0 7px;
      background-color: rgba(136, 14, 79, 0.7);
      border-radius: 0 3px 3px 0;
      color: white;
      overflow: hidden;
      border: 1px solid rgba(136, 14, 79, 0.7);
    }
  }
}

.single-filter {
  border: 2px solid transparent;
  box-shadow: 0 0 3px #880e4f;
  min-width: 300px;
  padding: 10px;

  &__label {
    font-weight: 700;
  }

  label {
    overflow: hidden;
  }

  .q-field--outlined .q-field__control {
    padding: 0 0 0 12px;

    .q-field__append {
      padding: 0 7px;
      background-color: rgba(173, 20, 87, 0.7);
      border-radius: 0 3px 3px 0;
      color: white;
      overflow: hidden;
      border: 1px solid rgba(173, 20, 87, 0.7);
    }
  }

  .innot {
    .q-field--outlined .q-field__control {
      .q-field__append {
        padding: 0 0;
        background-color: rgba(173, 20, 87, 0.7);
        border-radius: 0 3px 3px 0;
        color: white;
        overflow: hidden;
        border: 1px solid rgba(173, 20, 87, 0.7);
      }
    }
  }
}
</style>
