<template>
  <tr v-if="product" class="grid grid-cols-7 gap-x-1 max-w-2xl">
    <td class="flex w-full justify-center">
      <img :src="product.image" alt="" class="w-16 h-16">
    </td>

    <td class="col-span-3 ">
      <div :title="product.name" class="flex flex-col truncate">
        <span class="text-sm">{{ product.name }}</span>
        <p v-if="product.brand" class="text-gray-500 text-xs">
          {{ product.brand }}
        </p>
      </div>
    </td>


    <td class="text-center  py-1 ">
      {{ product.quantity }}
    </td>

    <td class="text-center py-1 ">
        <span class="text-xs lg:text-sm font-bold">
        {{ product.defaultPrice().value }}
      </span>
    </td>

    <td class="text-center">
      <div>
        <button @click="remove(product)"
                class="text-red-400 hover:text-red-600 transition">

          <svg class="h-4 w-4" fill="currentColor" viewBox="0 0 20 20"
               xmlns="http://www.w3.org/2000/svg">
            <path clip-rule="evenodd"
                  d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                  fill-rule="evenodd"/>
          </svg>
        </button>

        <slot name="actions"></slot>
      </div>
    </td>
  </tr>
</template>

<script>
import Product from "~/modules/shop/models/Product";
import {mapActions} from "vuex";

export default {
  props: {
    product: Product
  },

  methods: {
    ...mapActions({
      remove: 'shop/cart/remove',
    })
  }
}
</script>
