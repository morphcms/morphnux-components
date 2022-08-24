<template>
  <div class="flex flex-col relative space-y-20 px-0">
    <div class="flex flex-col space-y-4 items-center justify-center z-20">
      <badge v-if="category" size="xs" type="success"
             class="uppercase rounded font-bold text-xs'">
        {{ category }}
      </badge>
      <h2 class="text-center text-6xl font-bold max-w-6xl"
          :class="{'text-white' : image !== null}">
        {{ title }}
      </h2>
    </div>
    <div class="z-20 flex flex-col item-start" :class="image ? 'text-white' : 'text-gray-600'">
      <h3 class="text-lg font-medium max-w-[50rem]">
        {{ description }}
      </h3>
      <h4 v-if="subDescription" class="text-sm max-w-xl">
        {{ subDescription }}
      </h4>
    </div>
    <img v-if="image" :src="image" alt="background" class="absolute inset-0 h-full w-full z-10" />
  </div>
</template>

<script>
import Badge from "../../ui/Badge";
export default {
  name: "ArticleHeader",
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
  }
}
</script>
