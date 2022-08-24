<template>
  <client-only>
    <div id="command-search-modal" class="relative" :class="{'z-40': isOpen}" role="dialog" aria-modal="true">
      <transition
        enter-active-class="ease-out duration-300"
        enter-class="opacity-0"
        enter-to-class="opacity-100"

        leave-active-class="ease-in duration-200"
        leave-class="opacity-100"
        leave-to-class="opacity-0"
      >
        <div v-if="isOpen" class="fixed inset-0 bg-gray-500 bg-opacity-25 transition-opacity"></div>
      </transition>


      <div v-if="isOpen" class="fixed inset-0 z-30 overflow-y-auto p-4 sm:p-6 md:p-20">

        <transition
          enter-active-class="ease-out duration-300"
          enter-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
          enter-to-class="opacity-100 translate-y-0 sm:scale-100"

          leave-active-class="ease-in duration-200"
          leave-class="opacity-100 translate-y-0 sm:scale-100"
          leave-to-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        >

        </transition>

        <div
          v-click-away="onClickAway"
          v-if="isOpen"
          class="mx-auto max-w-xl transform divide-y divide-gray-100 overflow-hidden rounded-xl bg-white shadow-2xl ring-1 ring-black ring-opacity-5 transition-all">


          <div class="relative">
            <search-icon class="pointer-events-none absolute top-3.5 left-4 h-5 w-5 text-gray-400"/>

            <input
              id="command-input"
              @input="onInputChange"
              v-model="query"
              @keydown.esc="close"
              :placeholder="getPlaceholder()"
              type="search"
              role="combobox"
              aria-expanded="false"
              aria-controls="options"
              class="h-12 w-full border-0 bg-transparent pl-11 pr-4 text-gray-800 placeholder-gray-400 focus:ring-0 sm:text-sm"
            >
          </div>

          <!-- Default state, show/hide based on command palette state -->
          <command-loading-state v-if="loading"/>

          <!-- Results, show/hide based on command palette state -->
          <command-search-results
            :current-index="currentIndex"
            @select="select"
            v-else-if="hasResults && !loading"
            :results="results"
          />


          <!-- Empty state, show/hide based on command palette state -->
          <command-empty-state v-else-if="hasEmptyState"/>

          <command-default-state
            v-else-if="!hasQuery && !hasResults && !loading"
            :show-quick-actions="features.quickActions"
          />
          <command-search-footer v-if="features.footer"/>
        </div>
      </div>
    </div>
  </client-only>
</template>

<script>
import {SearchIcon} from "@vue-hero-icons/solid";

import CommandEmptyState from "@/components/CommandPalette/CommandEmptyState";
import CommandDefaultState from "@/components/CommandPalette/CommandDefaultState";
import CommandLoadingState from "@/components/CommandPalette/CommandLoadingState";
import CommandSearchResults from "@/components/CommandPalette/CommandSearchResults";
import {mapActions, mapGetters} from "vuex";
import CommandSearchFooter from "~/components/CommandPalette/CommandSearchFooter";
import CommandPalette from "~/modules/command-palette/services/CommandPalette";

export default {
  props: {
    placeholder: {
      type: String,
      default: null
    },
    shortcutKey: {
      type: String,
      default: '/'
    },
    clearOnSelect: {
      type: Boolean,
      default: true,
    },
    closeOnSelect: {
      type: Boolean,
      default: true,
    },
    minCharacters: {
      type: Number,
      default: 3,
    },
    debounce: {
      type: Number,
      default: 500,
    },
    clickAwayDebounce: {
      type: Number,
      default: 500
    }
  },

  components: {
    CommandSearchFooter,
    CommandSearchResults,
    CommandLoadingState,
    CommandDefaultState,
    CommandEmptyState,
    SearchIcon,
  },

  data() {
    return {
      query: null,
      timeoutId: null,
      canClickAway: false,
      clickAwayTimerId: null,
      currentIndex: null,
      features: {
        quickActions: false,
        footer: false,
        recentSearches: false,
        keyboardNavigation: false,
        toggleOnSwipe: false
      }
    }
  },

  mounted() {

    this.$nuxt.$on('command:open', this.open);


    if (this.shortcutKey !== null) {
      document.addEventListener('keyup', this.onKeyEvent);
    }
    if (this.features.toggleOnSwipe) {
      this.$nuxt.$on('swipe', this.toggleOnSwipe);
    }
  },

  unmounted() {
    this.$nuxt.$off('command:open', this.open);

    if (this.shortcutKey !== null) {
      document.removeEventListener('keyup', this.onKeyEvent);
    }

    if (this.features.toggleOnSwipe) {
      this.$nuxt.$off('swipe', this.toggleOnSwipe);
    }
  },

  computed: {
    ...mapGetters({
      isOpen: "command/getIsOpen",
      hasResults: 'command/getHasResults',
      results: 'command/getResults',
      hasEmptyState: 'command/getHasEmptyState',
      loading: 'command/getLoading',
      totalItems: 'command/getItemsCount',
    }),

    hasQuery() {
      return this.query && this.query.trim() !== '' && this.query.length >= this.minCharacters;
    },
  },

  methods: {
    ...mapActions({
      openModal: "command/open",
      closeModal: "command/close",
      toggleModal: "command/toggle",
      search: 'command/search',
      clear: 'command/clear',
    }),

    onInputChange() {
      clearTimeout(this.timeoutId)
      if (this.hasQuery) {
        this.timeoutId = setTimeout(() => this.search(this.query.trimLeft('/')), this.debounce);
        return;
      }

      if (!this.query) {
        this.onClear();
      }
    },

    addHashToLocation(searchQuery) {
      history.pushState(
        {},
        null,
        this.$route.path + '?search=' + encodeURIComponent(searchQuery)
      )
    },

    getPlaceholder() {
      if (this.placeholder !== null) {
        return this.placeholder;
      }

      return this.$t('command-palette:input-placeholder');
    },

    select(option) {

      this.$commandPalette.executeAction(option);

      if (this.closeOnSelect) {
        this.close();
      }

      this.$emit('select', option);

      if (this.clearOnSelect) {
        this.onClear();
      }
    },


    onClear() {
      this.query = null;
      this.clear();
      this.$emit('clear');
    },

    onClickAway() {
      if (this.isOpen && this.canClickAway) {
        this.close();
      }
    },

    open() {
      this.onClear();
      clearTimeout(this.clickAwayTimerId);
      this.clickAwayTimerId = setTimeout(() => this.canClickAway = true, this.clickAwayDebounce);

      if (this.isOpen) {
        return;
      }

      this.openModal();
      this.$emit('open');
      this.$nextTick(() => {
        document.getElementById('command-input').focus();
        this.query = '';
      })
    },

    close() {
      clearTimeout(this.clickAwayTimerId);
      this.canClickAway = false;

      if (!this.isOpen) {
        return;
      }
      this.closeModal();
      this.$emit('close');
    },

    toggle() {
      this.isOpen ? this.close() : this.open();
      this.$emit('toggle', this.isOpen);
    },

    onKeyEvent(e) {
      const key = e.key;

      if (key === this.shortcutKey) {
        this.toggleOnShortcut();
        return;
      }

      if (!this.features.keyboardNavigation) {
        return;
      }

      if (['ArrowUp', 'ArrowDown', 'Tab'].includes(key)) {
        switch (key) {
          case 'ArrowUp':
            switch (this.currentIndex) {
              case 0:
              case null:
                this.currentIndex = this.totalItems - 1;
                break;
              default:
                this.currentIndex--;
                break;
            }
            break;

          case 'Tab':
          case 'ArrowDown':
            switch (this.currentIndex) {
              case this.totalItems - 1:
              case null:
                this.currentIndex = 0;
                break;
              default:
                this.currentIndex++;
                break;
            }
        }
      }
    },

    toggleOnShortcut() {
      if (this.isOpen) {
        return;
      }

      if (['INPUT', 'TEXTAREA'].includes(document.activeElement.tagName)) {
        return;
      }

      this.open();
    },

    toggleOnSwipe(direction) {

      if (direction === 'bottom' && !this.isOpen) {
        this.open();
        return;
      }

      if (direction === 'top' && this.isOpen) {
        this.close();
      }
    }
  },


}
</script>
