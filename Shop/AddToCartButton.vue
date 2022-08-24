<template>
  <div>
    <ui-button v-if="canAddProduct(product)"
               @click.native="add(product)"
               size="xs"
               color="primary"
               class="bg-primary-blueDarker font-bold self-center mr-3"
    >


      <span>
        {{ addLabel }}
      </span>
    </ui-button>

    <ui-button v-else-if="hasProduct(product)"
               @click.native="remove(product)"
               size="xs"
               color="primary"
               class="bg-primary-blueDarker font-bold self-center mr-3">

      <span class="block group-hover:hidden text-green-500">
        <svg class="h-5 w-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
          <path clip-rule="evenodd"
                d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                fill-rule="evenodd"/>
        </svg>
      </span>

      <span class="hidden group-hover:block text-red-600">
          <svg class="h-5 w-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
            <path clip-rule="evenodd"
                  d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                  fill-rule="evenodd"/>
          </svg>
      </span>

      <span
        class="hidden group-hover:block text-xs lg:text-sm truncate text-red-400 group-hover:text-red-600 font-bold">
        {{ removeLabel }}
      </span>

    </ui-button>
  </div>
</template>

<script>
import {mapActions, mapGetters} from "vuex";
import UiButton from "@/ui/Button";
import Product from "~/modules/shop/models/Product";

export default {
  components: {UiButton},
  props: {
    product: Product,

    addLabel: {
      type: String,
      default: 'Add To Cart'
    },

    removeLabel: {
      type: String,
      default: 'Remove From Cart'
    }
  },

  computed: {
    ...mapGetters({
      selectedProducts: 'shop/cart/getProducts',
    })
  },

  methods: {
    ...mapActions({
      add: 'shop/cart/add',
      remove: 'shop/cart/remove',
    }),

    onClick(product) {
      if (this.canAddProduct(product)) {
        this.add(product);
        return;
      }

      if (this.hasProduct(product)) {
        this.remove(product);
      }
    },

    canAddProduct(currentProduct) {
      return true;
      // Add more logic in case needed when adding products to cart
      return this.hasProduct(currentProduct)
    },

    hasProduct(currentProduct) {
      return this.selectedProducts.filter((product) => product.id === currentProduct.id).length > 0;
    }
  }
}
</script>
