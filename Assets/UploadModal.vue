<template>
  <div>
    <ui-button size="sm" @click.native="show = true">
      Show Modal
    </ui-button>
    <portal to="modals">
      <generic-modal :show="show" @close="show = false" @submit="uploadFile">
        <template #title>
          <slot name="title">
            Upload Assets
          </slot>
        </template>
        <template>
          <dropzone
            id="dropzone"
            ref="dropzone"
            :destroyDropzone="true"
            :options="options"
            @vdropzone-files-added="filesAddedOrRemoved"
            @vdropzone-removed-file="filesAddedOrRemoved"
          />
        </template>
        <template #submitButton>
          Upload
        </template>
        <template #actions>
          <ui-button
            class="uppercase sm:ml-3"
            size="sm"
            type="button"
            @click.native="triggerDropzone"
          >
            Add more
          </ui-button>
        </template>
      </generic-modal>
    </portal>
  </div>
</template>

<script>
import Dropzone from "nuxt-dropzone";
import "nuxt-dropzone/dropzone.css";
import GenericModal from "~/components/Generic/Modal";
import UiButton from "~/ui/Button";

export default {
  components: {
    Dropzone,
    GenericModal,
    UiButton,
  },

  data() {
    return {
      show: false,
      options: {
        url: "https://httpbin.org/post",
        maxFilesize: 25,
        previewTemplate: this.layout()
      },
      numberOfFiles: 0
    };
  },
  methods: {
    async uploadFile() {
      let files = this.$refs.dropzone.getAcceptedFiles();
      const formData = new FormData();

      for (const file of files) {
        formData.append("file", file);

        await this.$axios.post("api/assets", formData, {
          headers: {
            "Content-Type": "multipart/form-data"
          }
        });

        formData.delete("file");
      }
      window.location.reload();
    },
    layout() {
      return (
        '<div class="flex flex-row justify-between bg-white mb-4 divide-x rounded">\n' +
        '  <div class="flex flex-row h-14 space-x-4">' +
        '   <img class="object-scale-down" data-dz-thumbnail />\n' +
        '   <div class="flex flex-col">\n' +
        "     <div><span data-dz-name></span></div>\n" +
        "     <div data-dz-size></div>\n" +
        "   </div>\n" +
        "  </div>\n" +
        '  <div class="self-center p-2" data-dz-remove>\n' +
        '   <svg id="removeButton" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 hover:opacity-50" fill="none" viewBox="0 0 24 24" stroke="currentColor">\n' +
        '     <path id="removeButton" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />\n' +
        "   </svg>\n" +
        "  </div>\n" +
        "</div>"
      );
    },
    filesAddedOrRemoved() {
      this.numberOfFiles = this.$refs.dropzone.getAcceptedFiles().length;
    },
    triggerDropzone() {
      document.getElementById("dropzone").click();
    }
  }
};
</script>

<style>
#dropzone {
  @apply bg-slate-100 hover:bg-slate-200;
}

#removeButton {
  @apply cursor-pointer;
}
</style>
