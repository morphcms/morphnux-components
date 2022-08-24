<template>
  <div class="relative">

    <div v-show="isCartOpen" class="fixed inset-0 z-40" @click="closeCart"></div>

    <transition
      enter-active-class="transition ease-out duration-200"
      enter-from-class="transform opacity-0 scale-95"
      enter-to-class="transform opacity-100 scale-100"
      leave-active-class="transition ease-in duration-75"
      leave-from-class="transform opacity-100 scale-100"
      leave-to-class="transform opacity-0 scale-95">

      <div  v-if="isCartOpen"  class="absolute flex-shrink-0 origin-top-right transform top-14 right-0 z-50">
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
              @click="closeCart"
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
      </div>
    </transition>

    <div class="flex justify-end">

      <button
        @click="toggleCart"
        class="relative inline-flex items-center space-x-1 text-sm font-semibold rounded-md text-slate-700  hover:text-slate-800 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-slate-500"
       >

        <shopping-cart-icon class="h-5 w-5 text-gray-400"/>

        <span class="hidden md:block">{{ $t('Cart')}}</span>

        <transition
          enter-active-class="transition ease-out duration-150"
          enter-from-class="transform opacity-0 scale-95"
          enter-to-class="transform opacity-100 scale-100"
          leave-active-class="transition ease-in duration-75"
          leave-from-class="transform opacity-100 scale-100"
          leave-to-class="transform opacity-0 scale-95">
          <badge v-if="cart.hasAny()" size="extra-xs" type="primary">
            {{ cart.count() }}
          </badge>
        </transition>
      </button>
    </div>
  </div>
</template>

<script>
import Cart from "~/components/Shop/Cart";

import {mapActions, mapGetters} from "vuex";
import {ShoppingCartIcon} from "@vue-hero-icons/outline"
import Badge from "@/ui/Badge";
import CartList from "~/components/Shop/CartList";
export default {
  components: {
    Cart,
    Badge,
    ShoppingCartIcon,
    CartList,
  },


  computed: {
    ...mapGetters({
      cart: 'shop/cart/getCart',
      isCartOpen: 'shop/cart/getIsOpen',
    }),
  },

  methods: {
    ...mapActions({
      toggleCart: 'shop/cart/toggle',
      closeCart: 'shop/cart/close',
    }),

  },

}
</script>
