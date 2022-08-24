<template>

  <div class="border border-blue-300 shadow rounded-md  max-w-sm w-full mx-auto">

    <div class="animate-pulse flex flex-col">
      <div class="p-2">
        <div class="rounded self-center bg-slate-300 h-52 w-full"></div>
      </div>

      <div class="p-4 flex flex-col space-y-4">
        <div class="flex-1 space-y-6 py-1">
          <div class="space-y-1">
            <skeleton-text size="xs" class="w-32"/>
            <skeleton-text size="xs" class="w-10"/>
          </div>

          <div class="flex items-center justify-between">
            <skeleton-text size="xs" class="w-14"/>
            <skeleton-text size="sm" class="w-20"/>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>

import SkeletonText from "~/ui/Skeletons/SkeletonText";

export default {
  components: {SkeletonText},
}
</script>
