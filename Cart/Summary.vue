<template>
  <div class="rounded shadow flex flex-col space-y-3 px-5 py-5">
    <div class="flex flex-col space-y-2 divide-y divide-blue-600">
      <div class="flex flex-col space-y-1">
        <div class="flex flex-row items-center justify-between">
          <p>Subtotal</p>

          <skeleton-text v-if="loading"  size="xs" class="w-28"/>

          <p v-else>{{ cart.subtotal }}</p>
        </div>

        <div class="flex flex-row items-center justify-between font-bold">
          <p>Discounts</p>
          <skeleton-text v-if="loading"  size="xs" class="w-20"/>
          <p v-else>{{ cart.discountTotal }}</p>
        </div>

        <div class="flex flex-row items-center justify-between">
          <p>Shipping</p>
          <skeleton-text v-if="loading"  size="xs" class="w-24"/>
          <p v-else>{{ cart.shippingTotal }}</p>
        </div>


        <div v-if="coupons.length > 0" class="flex flex-col space-y-1 py-2">
          <p>Coupons</p>
          <ul>
            <li v-for="coupon in coupons" class="flex justify-between items-center">
              <p class="text-sm text-gray-600">{{ coupon.title || coupon.code }} <span
                class="text-red-500">{{ coupon.discount.percentage }} off</span></p>
              <button @click="onRemove(coupon)" type="button">
                <x-icon class="w-4 h-4"/>
              </button>
            </li>
          </ul>

          <p v-if="coupons.length > 1">You save a total of <span class="font-bold">{{ coupons.reduce((partialSum, item) => partialSum + item.discount.raw, 0 ) }}%</span> </p>
        </div>

        <slot name="totals" />

      </div>

      <div class="flex flex-row items-center pt-3 justify-between text-2xl font-medium">
        <p>Total</p>
        <skeleton-text v-if="loading"  size="sm" class="w-32"/>
        <p v-else>{{ cart.total }}</p>
      </div>
    </div>

    <div>
      <ui-button @click.native="$emit('submit')" size="xl" :loading="loading" color="primary" class="justify-between w-full">

        <slot name="button">Checkout</slot>

        <credit-card-icon class="w-6 h-6"/>
      </ui-button>
    </div>

    <hr>

    <div>
      <img
        src="/images/Powered%20by%20Stripe%20-%20blurple.svg"
        alt="powered by Stripe"
        width="150"
      >
    </div>


    <slot />
  </div>
</template>

<script>
import UiButton from "@/ui/Button";
import {CreditCardIcon, XIcon} from "@vue-hero-icons/solid";
import Cart from "~/modules/shop/Cart";
import SkeletonText from "~/ui/Skeletons/SkeletonText";
import {mapActions, mapGetters} from "vuex";
import ValidationErrors from "~/ui/ValidationErrors";

export default {
  name: "Summary",
  components: {
    ValidationErrors,
    SkeletonText,
    UiButton,
    CreditCardIcon,
    XIcon,
  },

  props: {
    cart: Cart,
    loading: {
      type: Boolean,
      default: true,
    },
  },

  computed: {
    ...mapGetters({
      coupons: "shop/cart/getCoupons",
    })
  },
  methods: {
    ...mapActions({
      removeCoupon: "shop/cart/removeCoupon",
    }),

    onRemove(coupon){
      this.removeCoupon(coupon);
      this.$emit('update');
    }
  }
}
</script>
