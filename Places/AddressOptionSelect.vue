<template>
  <div>
    <div class="flex flex-col space-y-5">
      <address-card
        v-for="option in options"
        :location="option"
        :key="`address-option-${option.id}`"
        :is-selected="isSelected(option)"
        @click.native="select(option)">

        <template #title>{{ option.title }}</template>
        <template #description>{{ option.description }}</template>
      </address-card>

      <button
        @click="open"
        type="button"
              class="space-x-2 relative block w-full border-2 border-slate-300 border-dashed rounded-lg py-3 px-4 text-center hover:border-slate-400 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">

        <span class="inline-flex items-center text-sm font-medium text-slate-900">
          <plus-circle-icon  class="h-5 w-5 mr-2"/>
         {{ $t('Create a new address') }}
        </span>
      </button>

    </div>

    <portal to="modals">
      <generic-modal :show="showCreateModal" :closeable="true" @close="close" @submit="onSubmit">
        <template #title>
          {{ $t('Create Address')}}
        </template>

        <address-form-component ref="addressForm" @input="onUpdate"/>

      </generic-modal>
    </portal>
  </div>

</template>

<script>
import AddressCard from "~/components/Places/AddressCard";
import UiButton from "~/ui/Button";
import EmptyState from "~/ui/EmptyState";
import {PlusCircleIcon} from "@vue-hero-icons/outline";
import AddressFormComponent from "@/components/Places/AddressForm";
import GenericModal from "@/components/Generic/Modal";

export default {
  components: {EmptyState, AddressFormComponent, GenericModal, AddressCard, UiButton, PlusCircleIcon},
  props: {
    options: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      selectedOption: null,
      showCreateModal: false,
    }
  },

  created() {
    if (this.options.length > 0) {
      this.select(this.options[0]);
    }
  },

  methods: {
    isSelected(shippingOption) {
      if (!this.selectedOption) {
        return false;
      }
      return this.selectedOption.id === shippingOption.id;
    },

    select(shippingOption) {
      this.selectedOption = shippingOption;
      this.$emit('input', this.selectedOption.id);
    },

    open() {
      this.showCreateModal = true;
    },

    close() {
      this.$refs['addressForm'].reset();
      this.showCreateModal = false;
    },

    onUpdate(payload){
      this.$emit('update', payload);
    },

    async onSubmit() {
      try{
        await this.$refs['addressForm'].submit();
        this.close();
      } catch (e) {

      }

    },

  }
}
</script>
