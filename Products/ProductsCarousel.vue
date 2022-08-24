<template>
  <div class="flex flex-col space-y-1 h-full">
    <div class="flex flex-row justify-between items-center">
      <h2 class="text-3xl font-bold py-2">Most popular</h2>
      <div class="flex flex-row items-center space-x-3 text-primary-blue">
        <div class="rounded border border-primary-blue flex items-center justify-center p-1.5">
          <chevron-left-icon class="h-5 w-5"/>
        </div>
        <div class="rounded border border-primary-blue flex items-center justify-center p-1.5">
          <chevron-right-icon class="h-5 w-5"/>
        </div>
      </div>
    </div>
    <div class="grid grid-cols-3 grid-rows-1 gap-x-4">
      <product-card
        v-for="(product, productIdx) in products"
        :key="productIdx"
        :product="product"
        class="flex-1"
      />
    </div>
  </div>
</template>

<script>
import ProductCard from "@/components/Products/ProductCard";
import {ChevronLeftIcon, ChevronRightIcon} from "@vue-hero-icons/solid";

export default {
  name: "ProductsCarousel",
  components: {
    ProductCard,
    ChevronLeftIcon,
    ChevronRightIcon,
  },
  props: {
    products: {
      type: Array | Object,
      default: []
    },
  },
}
</script>
