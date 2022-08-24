<template>
  <button
    @click="openCommandInput"
    class="md:inline-flex md:items-center md:h-12 md:w-full md:border md:border-gray-200 md:px-2 md:py-1 md:shadow md:bg-transparent text-gray-400 focus:ring-0 sm:text-sm"
  >
    <search-icon class="h-5 w-5 text-gray-400 md:mr-2"/>
    <span class="hidden md:block">{{ $t('command-input:button') }}</span>
  </button>
</template>

<script>
import {SearchIcon} from "@vue-hero-icons/outline";
import TextField from "~/ui/Fields/Text";

export default {
  components: {
    TextField,
    SearchIcon,
  },

  methods: {
    openCommandInput() {
      this.$nuxt.$emit('command:open');
    }
  },

}
</script>
