<template>
  <div v-if="products.length > 0" class="w-full flex flex-col pb-10 pt-5 px-5 rounded-xl shadow-md space-y-2 overflow-hidden bg-white">
    <div class="w-full flex flex-row justify-between">
      <h2 class="font-medium text-xl">
        <slot name="name"></slot>
      </h2>
      <div class="flex flex-row items-center space-x-1 text-primary-blue">
        <div class="rounded border border-primary-blue flex items-center justify-center p-0.5">
          <chevron-left-icon class="h-5 w-5"/>
        </div>
        <div class="rounded border border-primary-blue flex items-center justify-center p-0.5">
          <chevron-right-icon class="h-5 w-5"/>
        </div>
      </div>
    </div>
    <div class="flex flex-row items-center pt-2" :class="{ 'space-x-12': size === 'small', 'space-x-20': size === 'big' }">
      <product-item v-for="(product, productIdx) in products" :key="'product-' + product.id" :product="product" :size="size"/>
    </div>
  </div>
</template>

<script>
import {ChevronLeftIcon, ChevronRightIcon} from "@vue-hero-icons/solid";
import ProductItem from "~/components/Products/ProductItem";


export default {
  name: "ProductsList",
  components: {
    ChevronLeftIcon,
    ChevronRightIcon,
    ProductItem
  },
  props: {
    products: {
      type: Array | Object,
      default: [],
    },
    size: {
      type: String,
      default: 'small',
    },
  },
}
</script>
