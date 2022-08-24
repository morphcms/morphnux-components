<template>
  <UiModal :closeable="closeable" :max-width="maxWidth" :show="show" @close="close">

    <div class="px-4 py-5 border-b border-slate-200 sm:px-6">
      <h3 class="text-lg leading-6 font-medium text-slate-900">
        {{ $t('Secured Payment') }}
      </h3>
    </div>

    <form @submit.prevent="handleSubmit" ref="paymentForm" id="payment-form">
      <div class="px-10 py-10">
        <div id="payment-element"></div>
        <div v-if="$shop.isLoading()" class="flex flex-col space-y-4 ">
          <div class="grid grid-cols-3 w-full gap-4 animate-pulse">
            <div class="h-16 bg-slate-200 col-span-1"></div>
            <div class="h-16 bg-slate-200 col-span-1"></div>
            <div class="h-16 bg-slate-200 col-span-1"></div>
          </div>
          <div class="grid grid-cols-4 w-full gap-4 animate-pulse">
            <div class="h-8 bg-slate-200 col-span-2"></div>
            <div class="h-8 bg-slate-200 col-span-1"></div>
            <div class="h-8 bg-slate-200 col-span-1"></div>
            <div class="h-8 bg-slate-200 col-span-full"></div>
          </div>
        </div>

        <InputError id="payment-message" :message="errorMessage"/>
      </div>


      <div class="bg-slate-50 px-4 py-3 sm:px-6 flex items-center justify-end space-x-4 w-full">
        <UiButton type="button" size="sm" @click.native="close">
          {{ $t('Cancel') }}
        </UiButton>

        <UiButton size="sm" color="primary" id="submit" :disabled="$shop.isLoading()">
          <SpinnerIcon v-if="$shop.isLoading()" class="animate-spin-fast h-4 w-4 mr-2"/>
          <span id="button-text">{{ $t('Pay') }} {{ price }}</span>
        </UiButton>
      </div>
    </form>
  </UiModal>
</template>

<script>
import ExtendsModal from "~/mixins/ExtendsModal";
import UiButton from "~/ui/Button";
import UiModal from '~/ui/Modal';
import SpinnerIcon from "~/ui/Icons/Spinner";
import InputError from "~/ui/InputError";
import CheckoutForm from "~/core/shop/Forms/CheckoutForm";
import HandlesStripeElements from "@/mixins/HandlesStripeElements";

export default {
  mixins: [ExtendsModal, HandlesStripeElements],
  components: {
    UiButton,
    UiModal,
    CheckoutForm,
    InputError,
    SpinnerIcon
  },

  watch: {
    async show(newValue) {
      if (newValue) {
        await this.initialize();
      }
    }
  },

  methods: {
    onPaymentSuccess(data) {
      this.$toast.success(this.$t('Order placed!'));
      this.close();
    }
  }

}
</script>
