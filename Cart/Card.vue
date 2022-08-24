<template>
  <div class="rounded shadow flex flex-col items-center px-10 py-5">

    <div class="grid grid-cols-2 items-center gap-x-24 w-full">
      <div class="flex flex-row items-center justify-between w-full space-x-4">

        <div class="h-36 w-36 flex-shrink-0 overflow-hidden rounded-md border border-gray-200">
          <img :src="product.image" :alt="product.name" class="h-full w-full object-cover object-center"/>
        </div>


        <div class="flex flex-col w-full">
          <p class="font-bold text-sm text-gray-600 truncate">{{ product.name }}</p>
          <p class="font-light text-sm text-gray-600">{{ product.brand }}</p>
        </div>


      </div>

      <div class="flex flex-row  items-center justify-end space-x-3">

        <ui-button @click.native="onRemove(product)" size="sm" rounded class="active:bg-primary-blueDarker active:text-white font-bold">
          -
        </ui-button>
        <ui-text :value="product.quantity" class="w-20 text-center"/>
        <ui-button @click.native="onAdd(product)" size="sm" rounded class="active:bg-primary-blueDarker active:text-white font-bold">
          +
        </ui-button>
      </div>
    </div>

    <div class="w-full justify-end flex flex-row items-center space-x-10 ">
      <p class="text-md">
        {{ product.defaultPrice().value }}<span class="text-sm">/pc</span>
      </p>

      <skeleton-text v-if="loading" size="lg" class="w-24" />

      <p v-if="!loading && product.total" class="text-2xl font-bold">
        {{ product.total.value }}
      </p>
    </div>
  </div>
</template>

<script>
import UiButton from "@/ui/Button";
import UiText from "@/ui/Fields/Text";
import Product from "~/modules/shop/Product";
import {mapActions} from "vuex";
import SkeletonText from "~/ui/Skeletons/SkeletonText";

export default {
  name: "Card",
  components: {
    SkeletonText,
    UiButton,
    UiText,
  },
  props: {
    product: Product,
    loading: Boolean,
  },

  methods: {
    ...mapActions({
      addProduct: 'shop/cart/add',
      removeProduct: 'shop/cart/remove',
    }),

    onAdd(product){
      this.addProduct(product);
      this.$emit('update');
    },

    onRemove(product){
      this.removeProduct(product);
      this.$emit('update');
    }
  },
}
</script>
