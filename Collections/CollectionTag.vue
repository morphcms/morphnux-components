<template>
  <nuxt-link :to="url">
    <badge size="xs" :type="collection.color || defaultColor" >
      {{ collection.name }}
    </badge>
  </nuxt-link>
</template>

<script>
import Collection from "@/modules/collection/models/Collection";
import Badge from "~/ui/Badge";

export default {
  components: {
    Badge
  },

  props: {
    collection: Collection,
    type: {
      type: String,
      default: 'default'
    },
    defaultColor: {
      type: String,
      default: 'default'
    }
  },

  computed: {
    url(){
      switch (this.type){
        case 'post':
          return '/blog/' + this.collection.slug;
        default:
        case 'default':
          return '/' + this.collection.slug;
      }
    }
  }
}
</script>
