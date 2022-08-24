<template>
  <dialog-modal :closeable="false" :show="isProcessingPayment">
    <template #content>
      <div class="flex">
        <div class="inline-flex items-center mx-auto">
          <spinner-icon class="h-5 w-5 animate-spin-fastest text-blue-400"/>
          <span class="ml-2 text-lg">Redirecting securely to the payment gateway...</span>
        </div>
      </div>
    </template>

    <template #footer>

    </template>
  </dialog-modal>
</template>

<script>
import DialogModal from "~/components/Ui/DialogModal";
import SpinnerIcon from "~/components/Ui/Icons/Spinner";
import {mapGetters} from "vuex";
export default {

  components: {
    DialogModal,
    SpinnerIcon
  },

  computed: {
    ...mapGetters({
      isProcessingPayment: 'shop/checkout/getLoading',
    }),
  }
}
</script>
