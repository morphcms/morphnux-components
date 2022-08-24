<template>
  <div class="rounded-xl shadow-recommended flex flex-col space-y-7 px-10 pb-10 pt-3 justify-between">
    <h2 class="font-bold text-2xl">
      {{ title }}
    </h2>
    <p class="text-gray-900 font-medium">
      {{ summary }}
    </p>
    <nuxt-link :to="link" class="self-end flex flex-row items-center space-x-2 hover:cursor-pointer">
      <p class="text-primary-blueDarker font-bold">
        Read More
      </p>
      <div class="rounded-full text-white bg-primary-blueDarker">
        <chevron-right-icon class="h-4 w-4"/>
      </div>
    </nuxt-link>
  </div>
</template>

<script>
import {ChevronRightIcon} from "@vue-hero-icons/solid";

export default {
  name: "RecommendedCard",
  components: {
    ChevronRightIcon,
  },
  props: {
    title: {
      type: String,
      default: '',
    },
    summary: {
      type: String,
      default: '',
    },
    link: {
      type: String,
      default: '#',
    }
  },
}
</script>
