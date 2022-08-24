<template>
  <div class="h-full">
    <div v-if="cart.hasAny()" class="flex flex-col justify-between px-2 h-full w-full">
      <table class="w-full">
        <thead class="">
        <tr class="grid grid-cols-7 w-full gap-0">
          <th class="col-span-1 py-2"></th>
          <th class="col-span-3 py-2"></th>
          <th class="col-span-1 py-2 text-center text-xs">{{ $t('QTY') }}</th>
          <th class="col-span-1 py-2 text-center text-xs">{{ $t('Price') }}</th>
          <th class="col-span-1 py-2 col-span-1"></th>
        </tr>
        </thead>

        <tbody class="flex flex-col overflow-y-scroll w-full" :class="height">

        <cart-item
          v-for="product in cart.items"
          :key="'cart-item-' + product.id"
          :product="product"
          class="flex w-full"
        />

        </tbody>
      </table>

      <div class="flex flex-col space-y-2">
        <hr>

        <div class="flex items-center justify-between px-2 py-3">
          <ui-button
            color="secondary"
            @click.native="clearAll">

            {{ $t('Clear All') }}

            <svg class="ml-4 h-5 w-5 text-red-400" fill="currentColor" viewBox="0 0 20 20"
                 xmlns="http://www.w3.org/2000/svg">
              <path clip-rule="evenodd"
                    d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                    fill-rule="evenodd"/>
            </svg>
          </ui-button>

          <ui-button as-link to="/cart" color="primary">
            {{ $t('View Cart') }}
            <svg xmlns="http://www.w3.org/2000/svg" class="ml-2 h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd"
                    d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-8.707l-3-3a1 1 0 00-1.414 1.414L10.586 9H7a1 1 0 100 2h3.586l-1.293 1.293a1 1 0 101.414 1.414l3-3a1 1 0 000-1.414z"
                    clip-rule="evenodd"/>
            </svg>
          </ui-button>
        </div>
      </div>

    </div>

    <div class="min-w-max" v-else>
      <alert>
        {{ $t('No products added.') }}
      </alert>
    </div>
  </div>
</template>

<script>
import {mapActions, mapGetters} from "vuex";
import Alert from "@/ui/Alert";
import CartItem from "@/components/Shop/CartItem";
import UiButton from "@/ui/Button";

export default {
  components: {
    Alert,
    CartItem,
    UiButton,
  },

  computed: {
    ...mapGetters({
      cart: 'shop/cart/getCart',
    }),
  },

  props: {
    height: {
      type: String,
      default: 'h-28'
    }
  },

  data() {
    return {
      unsubscribeCallback: null,
    }
  },

  created() {
    this.unsubscribeCallback = this.$store.subscribeAction({
      after: this.onActionTriggered
    });
  },

  methods: {
    ...mapActions({
      clearAll: 'shop/cart/clear',
      remove: 'shop/cart/remove',
    }),

    onActionTriggered(action) {
      const payload = action.payload;
      const type = action.type;

      switch (type) {
        case "shop/cart/add":
          this.emitEvents('add', payload);
          this.$toast.success(`Product added`);
          break;

        case "shop/cart/remove":
          this.emitEvents('remove', payload);
          break;

        case "shop/cart/clear":
          this.emitEvents('clear', payload);
          break;
      }
    },

    emitEvents(action, payload) {
      this.$emit(action, payload);
      this.$emit('update', {action, payload});
      this.$emit('input', this.cart.onlyIds());
    }
  },
  beforeDestroy() {
    this.unsubscribeCallback();
  },

}
</script>
