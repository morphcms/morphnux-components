<template>
  <div class="bg-white shadow-md border border-gray-200 rounded">
    <div class="flex justify-between border-b border-gray-200 px-4 py-5 sm:px-6">
      <div class="inline-flex items-center">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-1" fill="none" viewBox="0 0 24 24"
             stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"/>
        </svg>
        Cart
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

    <!-- Full Screen Dropdown Overlay -->


    <div class="flex flex-row divide-x-2 divide-gray-200 space-x-2 px-4 sm:px-6  min-w-max py-5">

      <div v-if="hasProducts" class="flex flex-col justify-between w-full">
        <table class="w-full">
          <thead class="">
          <tr class="flex w-full">
            <th class="w-1/6"></th>
            <th class="w-1/2"></th>
            <th class="text-center px-4 py-1 w-1/6">Price</th>
            <th class="text-center px-4 py-1 w-1/12">QTY</th>
            <th class="text-center px-4 py-1 w-1/6">Total</th>
            <th class="w-1/12"></th>
          </tr>
          </thead>

          <tbody class="flex flex-col overflow-y-scroll h-36 w-full">

          <cart-item
            v-for="product in products"
            :key="'product-' + product.id"
            :product="product"
            class="flex w-full"
          />

          </tbody>
        </table>
        <hr>
        <div class="flex items-center justify-between px-2 py-2">
          <div>
            Free Delivery
          </div>
          <ui-button
            color="secondary"
            @click.native="clearAll">

            Clear All

            <svg class="ml-4 h-5 w-5 text-red-400" fill="currentColor" viewBox="0 0 20 20"
                 xmlns="http://www.w3.org/2000/svg">
              <path clip-rule="evenodd"
                    d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                    fill-rule="evenodd"/>
            </svg>
          </ui-button>
        </div>
      </div>

      <div v-else >
        <alert>
          {{ $t('No products added.')}}
        </alert>
      </div>


      <div v-if="hasProducts" class="flex flex-col min-w-max space-y-2 justify-between px-2 w-full">
        <!--    Breakdown      -->
        <div class="flex flex-col h-24 overflow-y-auto w-full">
          <ul class="w-full">
            <li class="flex justify-between"><span>Subtotal:</span> 223.20 lei</li>
            <li class="flex justify-between"><span>Discount:</span><span class="text-red-500">-24 lei</span></li>
            <li class="flex justify-between"><span>Delivery:</span> -11.0 lei</li>
          </ul>
        </div>

        <hr>

        <div class="flex justify-between">
          <span>{{ $t('Total') }}:</span><span class="font-bold text-xl">{{ totalPrice }}</span>
        </div>

        <!--     Buttons     -->
        <div class="flex flex-col space-y-2">
          <ui-button color="primary" class="justify-between">
            Checkout
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-8.707l-3-3a1 1 0 00-1.414 1.414L10.586 9H7a1 1 0 100 2h3.586l-1.293 1.293a1 1 0 101.414 1.414l3-3a1 1 0 000-1.414z" clip-rule="evenodd" />
            </svg>
          </ui-button>
          <ui-button color="secondary" class="justify-between">
            {{ $t('Continue Shopping')}}

            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path d="M3 1a1 1 0 000 2h1.22l.305 1.222a.997.997 0 00.01.042l1.358 5.43-.893.892C3.74 11.846 4.632 14 6.414 14H15a1 1 0 000-2H6.414l1-1H14a1 1 0 00.894-.553l3-6A1 1 0 0017 3H6.28l-.31-1.243A1 1 0 005 1H3zM16 16.5a1.5 1.5 0 11-3 0 1.5 1.5 0 013 0zM6.5 18a1.5 1.5 0 100-3 1.5 1.5 0 000 3z" />
            </svg>
          </ui-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CartItem from '~/components/Shop/CartItem';
import {mapActions, mapGetters} from "vuex";
import Alert from "@/ui/Alert";
import UiButton from "@/ui/Button";

export default {
  components: {
    Alert,
    CartItem,
    UiButton,
  },

  computed: {
    ...mapGetters({
      totalPrice: 'shop/cart/getTotal',
      products: 'shop/cart/getProducts',
      hasProducts: 'shop/cart/hasProducts',
      isOpen: 'shop/cart/getIsOpen',
    }),
  },

  methods: {
    ...mapActions({
      close: 'shop/cart/close',
      clearAll: 'shop/cart/clear',
      remove: 'shop/cart/remove',
    }),

  },
}
</script>
