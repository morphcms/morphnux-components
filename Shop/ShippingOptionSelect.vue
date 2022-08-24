<template>
  <div class="mt-4 grid grid-cols-2 gap-x-3">
    <shipping-card
      v-for="(option, optionIdx) in options"
      :key="`shipping-option-${optionIdx}`"
      :is-selected="isShippingSelected(option)"
      @click.native="selectShipping(option)">

      <template #title>{{ option.name }}</template>
      <template #description>{{ option.description }}</template>
      <template #price>{{ option.price.value }}</template>
    </shipping-card>
  </div>
</template>

<script>
import ShippingCard from "@/components/Shop/ShippingCard";

export default {
  components: {
    ShippingCard,
  },

  props: {
    options: Array,
  },

  data() {
    return {
      selectedOption: null,
    }
  },

  created() {
    if (this.options.length > 0) {
      this.selectShipping(this.options[0]);
    }
  },

  methods: {
    isShippingSelected(shippingOption) {
      return this.selectedOption.id === shippingOption.id;
    },

    selectShipping(shippingOption) {
      this.selectedOption = shippingOption;
      this.$emit('input', this.selectedOption.id);
    },

  }
}
</script>
