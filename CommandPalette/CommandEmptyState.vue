<template>
  <div class="border-t border-gray-100 py-14 px-6 text-center text-sm sm:px-14">
    <emoji-sad-icon class="mx-auto h-6 w-6 text-gray-400"/>
    <p class="mt-4 font-semibold text-gray-900">
      {{ $t('command-palette:empty-state-title')}}
    </p>
    <p class="mt-2 text-gray-500">
      {{ $t("command-palette:empty-state-description") }}
    </p>
  </div>
</template>

<script>
import {EmojiSadIcon} from "@vue-hero-icons/outline";

export default {
  components: {
   EmojiSadIcon
  }
}
</script>
