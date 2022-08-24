<template>
  <ul
    id="options"
    role="listbox"
    class="max-h-80 scroll-py-10 scroll-pb-2 space-y-4 overflow-y-auto p-4 pb-2">

    <li v-for="(result, index) in results"
        :key="'command-group-' + index">

      <h2 v-if="result.group" class="text-xs font-semibold text-gray-900">
        {{ result.group }}
      </h2>

      <command-search-result
        v-for="(item, index) in result.items"
        :key="'command-group-' + index"
        :active="isActive(item, index)"
        :data="item"
        @click="$emit('select', item)"
      />
    </li>
  </ul>
</template>

<script type="ts">
import CommandSearchResult from "@/components/CommandPalette/CommandSearchResult";
import CommandQuickActions from "~/components/CommandPalette/CommandQuickActions.vue";

export default {
  props: {
    results: Array,
    currentIndex: {
      type: Number,
      default: null
    },
  },
  components: {
    CommandQuickActions,
    CommandSearchResult,
  },
  computed: {},
  methods: {
    isActive(item, index) {

      return this.currentIndex === index;
    }
  },
}
</script>
