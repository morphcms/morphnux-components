<template>
  <div class="rounded shadow p-5 pb-8 flex flex-col space-y-2 w-64">
    <div class="flex flex-row justify-between items-center">
      <h2 class="text-xl font-semibold">
        <slot></slot>
      </h2>
      <ui-button size="xs" color="default" @click.native="reset" class="uppercase font-bold">
        Reset
      </ui-button>
    </div>
    <div v-if="type === 'checkbox'" class="flex flex-col space-y-3">
      <div v-for="(filter, filterIdx) in filters" :key="slug + '-' + filterIdx"
           class="flex flex-row items-center space-x-2">
        <input type="checkbox" :value="filterIdx" v-model="selected"/>
        <label>{{ filter }}</label>
      </div>
    </div>
    <div v-else-if="type === 'range'" class="grid grid-cols-2 grid-rows-1 gap-x-1">
      <number v-model="min" @input="onInputRange" placeholder="Minimum" class="placeholder:text-xs"/>
      <number v-model="max" @input="onInputRange" placeholder="Maximum" class="placeholder:text-xs"/>
    </div>
  </div>
</template>

<script>
import UiButton from "@/ui/Button";
import Checkbox from "@/ui/Fields/Checkbox";
import {ChevronRightIcon} from "@vue-hero-icons/solid";
import Number from "@/ui/Fields/Number";

export default {
  name: "FilterCard.vue",
  components: {UiButton, Checkbox, ChevronRightIcon, Number},
  data() {
    return {
      selected: [],
      min: this.filters.min,
      max: this.filters.max
    }
  },
  props: {
    filters: {
      type: Array | String,
    },
    type: {
      type: String,
    },
    slug: {
      type: String,
    },
  },
  watch: {
    selected(newValue, oldValue) {
      this.onInput(newValue);
    },
  },

  methods: {
    onInput(filter) {
      this.$emit('input', filter);
    },
    onInputRange() {
      this.$emit('input', {min: this.min, max: this.max});
    },
    reset() {
      this.selected = [];
      if (this.type === 'range') {
        this.min = 0;
        this.max = null;
      }
    }
  },
}
</script>
