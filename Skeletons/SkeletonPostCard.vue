<template>

  <div class="border border-blue-300 shadow rounded-md w-80 md:w-96 w-full">

    <div class="animate-pulse flex flex-col">
      <div class="p-2">
        <div class="rounded self-center bg-slate-200 h-96 w-full"></div>
      </div>

      <div class="p-4 flex flex-col space-y-2">
        <skeleton-text size="sm" class="w-32"/>
        <skeleton-text size="xs" class=""/>
        <skeleton-text size="xs" class=""/>
        <skeleton-text size="xs" class=""/>
        <skeleton-text size="xs" class="w-1/2"/>
        <skeleton-text size="xs" class="w-full"/>
      </div>

    </div>
  </div>
</template>

<script>

import SkeletonText from "../morphnux/ui/Skeletons/SkeletonText";

export default {
  components: {SkeletonText},
}
</script>
