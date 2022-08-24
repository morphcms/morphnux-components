<template>
  <nav class="w-full flex flex-col space-y-4 py-7">

    <collapsable
      v-for="(link, linkIdx) in collections"
      :key="linkIdx"
      collapsed
    >
      <template #trigger="{ collapsed }">
        <div v-if="link.children.length > 0" class="inline-flex items-center w-full justify-between">
          {{ link.name }}
          <plus-icon class="h-5 w-5" v-if="collapsed"/>
          <minus-icon class="h-5 w-5" v-else/>
        </div>
        <nuxt-link v-else :to="`/${link.slug}`">
          {{ link.name }}
        </nuxt-link>
      </template>

      <template #default>
        <div v-if="link.children.length > 0" class="flex flex-col py-1 space-y-1 ml-2">
          <nuxt-link :to="'/' + link.slug" class="py-1 px-2">
            {{ $t('Parent Collection') }}
          </nuxt-link>

          <nuxt-link v-for="(child, index) in link.children" :key="index" :to="'/' + child.slug" class="py-1 px-2">
            {{ child.name }}
          </nuxt-link>
        </div>

      </template>

    </collapsable>
  </nav>
</template>

<script>
import Collapsable from "~/ui/Collapsable";
import {mapGetters} from "vuex";
import {PlusIcon, MinusIcon} from "@vue-hero-icons/outline";

export default {
  components: {
    Collapsable,
    PlusIcon,
    MinusIcon
  },
  computed: {
    ...mapGetters({
      collections: 'collections/getCollections',
    }),
  },
  methods: {
    getUrl(collection){
      let url = '/';
    }
  }
}
</script>
