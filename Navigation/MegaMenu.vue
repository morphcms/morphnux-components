<template>
  <div class="relative">
    <div class="w-full hidden md:flex justify-around px-40 py-7">

      <nav-item
        v-for="(link, linkIdx) in links"
        :key="linkIdx"
        :name="link.name"
        :children="link.children"
        :to="'/' + link.slug"
      />
    </div>

  </div>
</template>

<script>
import NavItem from "~/ui/NavItem";
import {mapActions, mapGetters} from "vuex";

export default {
  components: {
    NavItem,
  },
  props: {
    align: {
      default: 'right'
    },
    contentClasses: {
      default: () => ['py-1', 'bg-white']
    }
  },

  data() {
    return {
      open: false,
      selectedCollection: {},
    }
  },

  mounted() {
    document.addEventListener('keydown', this.closeOnEscape);
  },
  unmounted() {
    document.removeEventListener('keydown', this.closeOnEscape)
  },

  computed: {
    ...mapGetters({
      links: 'collections/getCollections',
    }),

    closeOnEscape(e) {
      if (open.value && e.keyCode === 27) {
        open.value = false
      }
    },
    alignmentClasses() {
      if (this.align === 'left') {
        return 'origin-top left-0'
      } else if (this.align === 'right') {
        return 'origin-top right-0'
      } else {
        return 'origin-top'
      }
    },
  },
}
</script>
