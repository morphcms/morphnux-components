<template>
  <div>
    <div class="border-t border-gray-100 py-14 px-6 text-center text-sm sm:px-14">
      <!-- Heroicon name: outline/globe -->
      <globe-icon class="mx-auto h-6 w-6 text-gray-400"/>
      <p class="mt-4 font-semibold text-gray-900">{{ $t('command-palette:default-search-title') }}</p>
      <p class="mt-2 text-gray-500">{{ $t('command-palette:default-search-description') }}</p>
    </div>

    <ul v-if="showQuickActions" class="max-h-80 scroll-py-2 divide-y divide-gray-500 divide-opacity-20 overflow-y-auto">
      <command-quick-actions/>
    </ul>
  </div>

</template>

<script>
import {GlobeIcon} from "@vue-hero-icons/outline";
import CommandQuickActions from "~/components/CommandPalette/CommandQuickActions";

export default {
  props: {
    showQuickActions: Boolean
  },
  components: {
    CommandQuickActions,
    GlobeIcon,

  }
}
</script>
