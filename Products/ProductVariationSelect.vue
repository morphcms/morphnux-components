<template>
  <div v-if="variationSelectOptions.length > 1">
    <h2 class="uppercase font-bold text-xl">Select Variation</h2>

    <ul v-if="variationSelectOptions.length <= selectThreshold"
         class="flex flex-row space-x-2 items-center">
      <li v-for="(name, slug) in variations" :key="slug">
        <nuxt-link v-if="slug !== variationSlug" :to="`/product/${slug}`">
          <ui-badge size="sm"
                    class="bg-white border border-black hover:border-blue-500 hover:bg-blue-200">
            {{ name }}
          </ui-badge>
        </nuxt-link>
        <ui-badge v-else size="sm"
                  type="primary">
          {{ name }}
        </ui-badge>
      </li>
    </ul>

    <div v-else class="flex w-full">
      <ui-select :value="variationSlug" :options="variationSelectOptions" @input="onSelect" class="w-full"/>
    </div>
  </div>
</template>

<script>
import UiBadge from "@/ui/Badge";
import UiSelect from "@/ui/Fields/Select";

export default {
  components: {
    UiBadge,
    UiSelect,
  },
  props: {
    variationSlug: String,
    selectThreshold: {
      type: Number,
      default: 5
    }
  },

  data() {
    return {
      loading: false,
      variations: {},
    }
  },

  async created() {
    await this.fetchVariations();
  },

  computed: {
    variationSelectOptions() {
      if (this.variations.length === 0) {
        return [];
      }

      return Object.keys(this.variations).map((slug) => {
        return {
          label: this.variations[slug],
          value: slug,
        }
      })
    }
  },

  methods: {
    async fetchVariations() {
      this.loading = true
      const {variations} = (await this.$axios.get(`/api/shop/options/${this.variationSlug}`).finally(() => this.loading = false)).data;

      this.variations = variations;
    },
    onSelect(slug) {
      this.$router.push(`/product/${slug}`);
    }
  }
}
</script>
