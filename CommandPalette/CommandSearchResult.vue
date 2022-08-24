<template>
  <ul @click="$emit('click')" class="group -mx-4 mt-2 text-sm text-gray-700">
    <li role="option"
        tabindex="-1"
        :class="{'bg-primary-blue text-white hover': active, 'hover:bg-gray-100': !active}"
        class="group flex cursor-default select-none items-center px-4 py-2  transition">

      <component
        v-if="currentIcon"
        :is="currentIcon"
        class="h-6 w-6 flex-none "
        :class="[active ? 'text-text-gray-200' : 'text-gray-400']"
      />

      <span class="ml-3 flex-auto truncate"> {{ data.title }}</span>

      <span
        v-if="data.action"
        class="ml-3 hidden group-hover:block flex-none" :class="[active ? 'text-gray-200' : 'text-gray-400']">
        <chevron-right-icon v-if="data.action.type === 'preview'"  class="w-3 h-3"/>
        <reply-icon v-else class="w-3 h-3" />
      </span>
    </li>

    <li v-if="data.description" class="cursor-default select-none px-4 py-2" id="option-2" role="option" tabindex="-1">
      {{ data.description }}
    </li>
  </ul>
</template>

<script>
import SearchResult from "~/modules/command-palette/models/SearchResult";
import { ReplyIcon, ChevronRightIcon} from "@vue-hero-icons/outline";

export default {
  props: {
    data: SearchResult,
    active: Boolean,
  },

  components: {
    ReplyIcon,
    ChevronRightIcon
  },

  created() {
    this.loadIcon();
  },

  data() {
    return {
      currentIcon: null,
    }
  },

  methods: {
    loadIcon() {
      const icons = import("@vue-hero-icons/outline");
      icons.then((data) => {
        this.currentIcon = data[this.data.icon];
      });
    }
  },
}
</script>
