<template>
  <div class="bg-white shadow-md border border-gray-200 rounded">
    <div class="flex justify-between border-b border-gray-200 px-4 py-5 sm:px-6">
      <div class="inline-flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-1" fill="none" viewBox="0 0 24 24"
             stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"/>
        </svg>
        {{ $t('Cart') }}
      </div>

      <button
        @click="close"
        class="text-gray-400 hover:text-gray-600 cursor-pointer transition">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd"
                d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                clip-rule="evenodd"/>
        </svg>
      </button>
    </div>


    <cart-list class="min-w-max"/>
  </div>


</template>

<script>
import CartItem from '~/components/Shop/CartItem';
import {mapActions, mapGetters} from "vuex";
import Alert from "@/ui/Alert";
import UiButton from "@/ui/Button";
import CartList from "~/components/Shop/CartList";

export default {
  components: {
    CartList,
    Alert,
    CartItem,
    UiButton,
  },

  computed: {
    ...mapGetters({
      isOpen: 'shop/cart/getIsOpen',
    }),
  },

  methods: {
    ...mapActions({
      close: 'shop/cart/close',
    }),

  },
}
</script>
