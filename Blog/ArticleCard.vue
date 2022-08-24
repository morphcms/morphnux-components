<template>
  <div class="flex flex-col relative"
       :class="{'items-start justify-around px-10': large, 'items-center text-center justify-between py-10 px-5': !large, 'rounded shadow h-72': !header, 'space-y-20 px-0': header }">
    <div class="flex flex-col space-y-4 items-center justify-center z-20">
      <badge v-if="category" :size="large ? 'customize' : 'xs'" type="success"
             class="uppercase rounded font-bold" :class="large ? 'px-10 py-1.5 text-[0.65rem]' : 'text-xs'">
        {{ category.name }}
      </badge>
      <h2 class="text-center"
          :class="{'text-white' : image !== null, 'text-6xl font-bold max-w-6xl': header, 'text-lg max-w-xs font-bold ': !header}">
        {{ title }}
      </h2>
    </div>
    <div class="z-20 flex flex-col item-start" :class="image ? 'text-white' : 'text-gray-600'">
      <h3
        :class="{'text-xl max-w-2xl font-bold': large, 'text-md font-medium': !large && !header, 'text-lg font-medium max-w-[50rem]': header}">
        {{ description }}
      </h3>
      <h4 v-if="subDescription" class="text-sm max-w-xl">
        {{ subDescription }}
      </h4>
    </div>
    <img v-if="image" :src="image" alt="background" class="absolute inset-0 h-full w-full z-10">
  </div>
</template>

<script>
import Badge from "@/ui/Badge";

export default {
  name: "ArticleCard",
  components: {Badge},
  props: {
    category: {
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
    subDescription: {
      type: String,
      default: null,
    },
    image: {
      type: String,
      default: null,
    },
    large: {
      type: Boolean,
      default: false,
    },
    header: {
      type: Boolean,
      default: false,
    },
  },
}
</script>
