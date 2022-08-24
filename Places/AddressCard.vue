<template>
  <div
    :class="baseClass"
    class="flex flex-col justify-between space-y-5 bg-white shadow px-4 py-3 rounded-md">

    <div class="relative w-full">
      <h3 class="text-md"><slot name="title" /></h3>
      <p class="text-gray-400"><slot name="description" /></p>


      <div v-if="isSelected" class="absolute top-0 right-0">
        <check-circle-icon class="w-5 h-5 text-primary-blueDarker"/>
      </div>


    </div>

    <div class="flex flex-row space-x-3">
      <ui-badge v-if="location.shipping_default" class="self-start">
        {{ $t('Shipping') }}
      </ui-badge>
      <ui-badge v-if="location.billing_default" class="self-start">
        {{ $t('Billing') }}
      </ui-badge>
    </div>
  </div>
</template>

<script>
import {CheckCircleIcon} from "@vue-hero-icons/solid";
import UiBadge from "~/ui/Badge"

export default {
  components: {
    CheckCircleIcon,
    UiBadge,
  },

  props: {
    isSelected: Boolean,
    location: {
      type: Object,
      default: null,
    },
    dashboard: {
      type: Boolean,
      default: false,
    }
  },

  computed: {
    baseClass(){
      if(this.isSelected){
        return 'border border-primary-blueDarker';
      }

      return 'border border-gray-300 cursor-pointer hover:border-primary-blueHue transition ';
    }
  },
}
</script>
