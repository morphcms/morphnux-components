<template>
  <div class="flex" :class="{ 'flex-row space-x-2': !isBig, 'flex-col items-center space-y-2': isBig}">
    <img :src="product.image" :alt="product.name" :class="{ 'h-24 w-24': !isBig, 'h-48 w-48': isBig,}"/>
    <div class="flex flex-col" :class="{ 'w-44 justify-around': !isBig, 'h-20 justify-between text-center space-y-4': isBig,}">
      <p class="text-gray-600 max-w-xs">{{ product.name }}</p>
      <p v-if="product.prices.length > 0" class="font-bold text-sm"> {{ product.prices[0].value }} </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductItem",
  props: {
    product: {
      type: Object,
      default: null
    },
    size: {
      type: String,
      default: 'small',
    }
  },
  computed: {
    isBig() {
      switch (this.size) {
        case 'small':
          return false;
        case 'big':
          return true;
      }
    }
  },
}
</script>
