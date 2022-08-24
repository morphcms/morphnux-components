<template>
  <div class="rounded-xl shadow-figma flex flex-col">
    <nuxt-link :to="'/blog/article/' + slug">
      <img :src="image" alt="cover"/>
    </nuxt-link>
    <div class="flex flex-col px-10 h-full justify-between space-y-5 py-5">
      <nuxt-link :to="'/blog/article/' + slug" class="flex flex-col space-y-2">
        <h1 class="text-lg font-bold hover:underline">{{ title }}</h1>
        <p class="text-base">{{ description }}</p>
      </nuxt-link>
      <div class="flex flex-row items-center">
        <ui-badge v-for="(category, categoryIdx) in categories" :key="categoryIdx" class="uppercase font-bold text-xs"
                  type="customize"
                  :color="category.color"
        >
          {{ category.name }}
        </ui-badge>
      </div>
    </div>
  </div>
</template>

<script>
import UiBadge from "~/ui/Badge"

export default {
  name: 'post-card',
  components: {UiBadge},
  props: {
    image: {
      type: String,
      default: null,
    },
    title: {
      type: String,
      default: '',
    },
    description: {
      type: String,
      default: '',
    },
    categories: {
      type: Array | Object,
      default: [],
    },
    slug: {
      type: String,
      default: '#',
    }
  },
}
</script>
