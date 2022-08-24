<template>
  <div class="border-t border-gray-100 py-14 px-6 text-center text-sm sm:px-14">
    <svg class="animate-spin-fastest mx-auto h-6 w-6 text-gray-400"
         xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
      <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
      <path class="opacity-75" fill="currentColor"
            d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
    </svg>
    <p class="mt-4 font-semibold text-gray-900">
      {{ $t('command-palette:loading') }}
    </p>
  </div>
</template>

<script>
import {GlobeIcon} from "@vue-hero-icons/outline";

export default {
  components: {
    GlobeIcon,
  }
}
</script>
