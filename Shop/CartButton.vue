<template>
  <button
    @click="$emit('click')"
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
</template>

<script>
import Cart from "@/components/Shop/Cart";
import Badge from "@/ui/Badge";
import {ShoppingCartIcon} from "@vue-hero-icons/outline";
import {mapActions, mapGetters} from "vuex";

export default {
  components: {
    Cart,
    Badge,
    ShoppingCartIcon,
  },

  computed: {
    ...mapGetters({
      cart: 'shop/cart/getCart',
    }),
  },

}
</script>
