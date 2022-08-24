<template>
  <div>
    <div v-if="link.children.length > 0" class="inline-flex items-center w-full justify-between">
      {{ link.name }}
      <plus-icon class="h-5 w-5" v-if="collapsed"/>
      <minus-icon class="h-5 w-5" v-else/>
    </div>
    <nuxt-link v-else :to="`/${link.slug}`">
      {{ link.name }}
    </nuxt-link>
  </div>
</template>

<script>
import {MinusIcon, PlusIcon} from "@vue-hero-icons/outline";

export default {
  props: {
    collection: Object,
  },
  components: {
    PlusIcon,
    MinusIcon
  }

}
</script>
