<template>
  <div class="border border-blue-300 shadow rounded-md p-4 max-w-sm w-full mx-auto">

    <div class="animate-pulse flex flex-col space-y-4">

      <div class="grid grid-cols-2 gap-4">
        <skeleton-text class="col-span-1" size="sm"/>
      </div>

      <div class="flex-1 space-y-3 py-1 w-full">

        <div class="flex items-center space-x-4">
          <div class="w-5 h-5 rounded bg-slate-200 rounded" />
          <skeleton-text size="sm" />
        </div>

        <div class="flex items-center space-x-4">
          <div class="w-5 h-5 rounded bg-slate-200 rounded" />
          <skeleton-text size="sm" />
        </div>

        <div class="flex items-center space-x-4">
          <div class="w-5 h-5 rounded bg-slate-200 rounded" />
          <skeleton-text size="sm" />
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
