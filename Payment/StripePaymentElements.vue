<template>
  <Panel>
    <form @submit.prevent="handleSubmit" ref="paymentForm" id="payment-form" class="flex flex-col space-y-4">
      <div id="payment-element"></div>

      <div class="flex w-full justify-end">
        <UiButton color="primary" id="submit">
          <SpinnerIcon v-if="$shop.isLoading()" class="animate-spin-fast h-4 w-4 mr-2"/>
          <span id="button-text">{{ $t('Pay') }} {{ price }}</span>
        </UiButton>
      </div>

      <InputError id="payment-message" :message="errorMessage"/>
    </form>
  </Panel>
</template>

<script>
import Panel from "~/ui/Panel";
import SpinnerIcon from "~/ui/Icons/Spinner";
import UiButton from "~/ui/Button";
import InputError from "~/ui/InputError";
import CheckoutForm from "~/core/shop/Forms/CheckoutForm";

export default {
  components: {InputError, Panel, SpinnerIcon, UiButton},
  props: {
    price: String,
    checkoutForm: CheckoutForm
  },

  data() {
    return {
      elements: [],
      errorMessage: null
    }
  },

  async created() {
    await this.initialize();
  },

  methods: {
    async initialize() {
      await this.$shop.fetchPaymentIntent(this.checkoutForm).then((data) => {
        this.elements = this.$stripe.elements({
          clientSecret: data.client_secret
        });

        const paymentElement = this.elements.create("payment");
        paymentElement.mount("#payment-element");
      });
    },

    async handleSubmit() {
      const elements = this.elements;

      const returnUrl = this.$config.baseUrl + '/checkout/status';

      try {
        const {paymentIntent, error} = await this.$stripe.confirmPayment({
          elements,
          confirmParams: {
            // Make sure to change this to your payment completion page
            return_url: returnUrl,
          },
          redirect: "if_required"
        });



        // This point will only be reached if there is an immediate error when
        // confirming the payment. Otherwise, your customer will be redirected to
        // your `return_url`. For some payment methods like iDEAL, your customer will
        // be redirected to an intermediate site first to authorize the payment, then
        // redirected to the `return_url`.
        if (error) {
          if (error.type === "card_error" || error.type === "validation_error") {
            this.$toast.error(error.message);
          } else {
            this.$toast.error(this.$t("An unexpected error occurred."));
          }
        } else {
          const formData = this.checkoutForm;
          formData.payment_method = paymentIntent.payment_method;
          await this.$shop.submitCheckout(formData).then((data) => {
            this.$toast.success('Order placed!');
          }).catch(e => this.$toast.error(e.message));
        }

      } catch (e) {
        console.log(elements);
        console.error(e);
      }
    }

  }

}
</script>
