<template>
  <div class="rounded shadow flex flex-col space-y-3 px-5 py-5">
    <label for="coupon_code" class="text-md">Coupon</label>

    <div class="flex space-x-2 w-full items-center">

      <text-field id="coupon_code" v-model="coupon" placeholder="Enter your coupon/voucher code"/>

      <ui-button @click.native="submit" size="sm" :loading="loading" color="primary" class="flex-shrink-0">
        <slot name="button">Redeem</slot>
      </ui-button>

    </div>
  </div>
</template>

<script>
import UiButton from "@/ui/Button";
import {CreditCardIcon, XIcon} from "@vue-hero-icons/solid";
import SkeletonText from "~/ui/Skeletons/SkeletonText";
import TextField from "@/ui/Fields/Text";
import {mapActions, mapGetters} from "vuex";
import FieldWrapper from "@/ui/FieldWrapper";
import Coupon from "@/modules/shop/Coupon";

export default {
  name: "Summary",
  components: {
    FieldWrapper,
    TextField,
    SkeletonText,
    UiButton,
    XIcon,
    CreditCardIcon,
  },

  data() {
    return {
      loading: false,
      coupon: null,
    }
  },

  computed: {
    ...mapGetters({
      coupons: "shop/cart/getCoupons",
    }),
  },
  methods: {
    ...mapActions({
      addCoupon: "shop/cart/addCoupon",
    }),

    submit() {
      this.loading = true;
      this.$axios.post('/api/coupons/check', {coupon: this.coupon})
        .then(({data}) => {
          this.addCoupon(new Coupon(data.coupon));
          this.coupon = null;
          this.$emit('update');
          this.$toast.success('Coupon redeemed.');
        }).catch((e) => {
        if (e.response.status === 422) {
          this.$toasted.error('The given coupon is invalid or expired.')
        } else {
          this.$toasted.error(e.message);
        }
      })
        .finally(() => this.loading = false)

    }
  }

}
</script>
