<template>
  <li class="p-2">
    <h2 class="sr-only">Quick actions</h2>
    <ul class="text-sm text-gray-400">
      <!-- Active: "bg-gray-800 text-white" -->
      <li class="group flex cursor-default select-none items-center rounded-md px-3 py-2">
        <!-- Active: "text-white", Not Active: "text-gray-500" -->
        <document-add-icon class="h-6 w-6 flex-none text-gray-500"/>

        <span class="ml-3 flex-auto truncate">Add new file...</span>
        <span class="ml-3 flex-none text-xs font-semibold text-gray-400"><kbd class="font-sans">⌘</kbd><kbd class="font-sans">N</kbd></span>
      </li>
      <li class="group flex cursor-default select-none items-center rounded-md px-3 py-2">

        <folder-add-icon class="h-6 w-6 flex-none text-gray-500"/>

        <span class="ml-3 flex-auto truncate">Add new folder...</span>
        <span class="ml-3 flex-none text-xs font-semibold text-gray-400"><kbd class="font-sans">⌘</kbd><kbd class="font-sans">F</kbd></span>
      </li>
      <li class="group flex cursor-default select-none items-center rounded-md px-3 py-2">

        <hashtag-icon class="h-6 w-6 flex-none text-gray-500"/>

        <span class="ml-3 flex-auto truncate">Add hashtag...</span>
        <span class="ml-3 flex-none text-xs font-semibold text-gray-400"><kbd class="font-sans">⌘</kbd><kbd class="font-sans">H</kbd></span>
      </li>
      <li class="group flex cursor-default select-none items-center rounded-md px-3 py-2">

        <tag-icon class="h-6 w-6 flex-none text-gray-500" />

        <span class="ml-3 flex-auto truncate">Add label...</span>
        <span class="ml-3 flex-none text-xs font-semibold text-gray-400"><kbd class="font-sans">⌘</kbd><kbd class="font-sans">L</kbd></span>
      </li>
    </ul>
  </li>
</template>

<script>
import {TagIcon, HashtagIcon, FolderAddIcon, DocumentAddIcon} from "@vue-hero-icons/outline";

export default {
  components: {
    TagIcon,
    HashtagIcon,
    FolderAddIcon,
    DocumentAddIcon
  }
}
</script>
