<template>
  <div
    :class="baseClass"
    class="flex flex-col space-y-5 bg-white shadow px-4 py-3 rounded-md">

    <div class="relative w-full">
      <h3 class="text-md"><slot name="title" /></h3>
      <p class="text-gray-400"><slot name="description" /></p>

      <div v-if="isSelected" class="absolute top-0 right-0">
        <check-circle-icon class="w-5 h-5 text-primary-blueDarker"/>
      </div>
    </div>

    <div>
      <span class="font-semibold"><slot name="price"></slot></span>
    </div>
  </div>
</template>

<script>
import {CheckCircleIcon} from "@vue-hero-icons/solid";

export default {
  components: {
    CheckCircleIcon,
  },

  props: {
    isSelected: Boolean
  },

  computed: {
    baseClass(){
      if(this.isSelected){
          return 'border-2 border-primary-blueDarker';
      }

      return 'border border-gray-300 cursor-pointer hover:border-primary-blueHue transition ';
    }
  },
}
</script>
