<template>
  <div class="flex flex-col lg:flex-row lg:justify-between">
    <div class="flex flex-col">
      <span class="text-md truncate">{{ product.name }}</span>
      <p v-if="$slots['description']" class="text-gray-500 text-sm">
        <slot name="description" />
      </p>
    </div>

    <div class="flex flex-col space-y-2 lg:flex-row lg:space-y-0 lg:space-x-4 lg:justify-between">
      <span v-if="product.price_discounted" class="line-through text-xs lg:text-sm">
        £{{ product.price }}
      </span>

      <span class="font-bold text-sm lg:text-md">
        £{{ product.price_discounted || product.price }}
      </span>

      <slot name="actions"></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "ListItem",
  props: ['product'],
}
</script>
