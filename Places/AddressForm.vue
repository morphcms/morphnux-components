<template>
  <form-section>
    <template #form>
      <field-wrapper :error="form.getError('fist_name')" class="col-span-3">
        <template #label>{{ $t('First Name') }}</template>
        <text-field v-model="form.first_name"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('last_name')" class="col-span-3">
        <template #label>{{ $t('Last Name') }}</template>
        <text-field v-model="form.last_name"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('company_name')" class="col-span-full">
        <template #label>{{ $t('Company') }} ({{ $t('Optional')}})</template>
        <text-field v-model="form.company_name"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('line_one')" class="col-span-full">
        <template #label>{{ $t('Address Line 1')}}</template>
        <text-field v-model="form.line_one"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('line_two')" class="col-span-full">
        <template #label>{{ $t('Address Line 2')}} ({{ $t('Optional')}})</template>
        <text-field v-model="form.line_two"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('country_id')" class="col-span-3">
        <template #label>{{ $t('Country') }}</template>

        <searchable
          v-model="form.country_id"
          label-field-name="name"
          endpoint="/api/places/countries/search"
        />
      </field-wrapper>

      <field-wrapper :error="form.getError('city_id')" class="col-span-3">
        <template #label>{{ $t('City') }}</template>

        <searchable
          v-if="form.country_id"
          v-model="form.city_id"
          label-field-name="name"
          :endpoint="`/api/places/states/search?country=${form.country_id}`"
        />

        <p v-else class="mt-2">{{ $t('You must select a country first.')}}</p>

      </field-wrapper>

      <field-wrapper :error="form.getError('state')" class="col-span-3">
        <template #label>{{ $t('State / Province')}} ({{ $t('Optional')}})</template>
        <text-field v-model="form.state"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('postcode')" class="col-span-3">
        <template #label>{{ $t('Postal Code')}}</template>
        <text-field v-model="form.postcode"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('contact_phone')" class="col-span-full">
        <template #label>{{ $t('Contact Phone')}}</template>
        <text-field v-model="form.contact_phone"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('delivery_instructions')" class="col-span-full">
        <template #label>{{ $t('Delivery Instructions')}} ({{ $t('Optional') }})</template>
        <textarea-field v-model="form.delivery_instructions"/>
      </field-wrapper>

      <field-wrapper :error="form.getError('shipping_default')" class="col-span-3">
        <checkbox v-model="form.shipping_default">{{ $t('Make default for shipping')}}</checkbox>
      </field-wrapper>

      <field-wrapper :error="form.getError('billing_default')" class="col-span-3">
        <checkbox v-model="form.billing_default">{{ $t('Make default for billing')}}</checkbox>
      </field-wrapper>

    </template>
  </form-section>
</template>

<script>
import FormSection from "@/ui/FormSection";
import FieldWrapper from "@/ui/FieldWrapper";
import TextField from "@/ui/Fields/Text";
import TextareaField from "@/ui/Fields/Textarea";
import Checkbox from "@/ui/Fields/Checkbox";
import {HomeIcon} from "@vue-hero-icons/solid";
import Searchable from "~/ui/Fields/Searchable";
import AddressForm from "~/modules/places/forms/AddressForm";
import Address from "~/modules/places/Address";


export default {
  components: {Searchable, HomeIcon, TextField, TextareaField, Checkbox, FieldWrapper, FormSection},
  props: {
    location: {
      type: Object,
      default: {},
    },
  },
  data(){
    return {
      form: this.$form(new AddressForm(this.location)),
    }
  },

  methods: {
    async reset(){
      this.form.reset();
    },

    async submit() {
      try {
        await this.form.post('/api/places/addresses').then(({data}) => {
          this.$emit('input', new Address(data));
          this.form.reset();
        });
      } catch (e) {

      }

    },

    async update(id) {
      try {
        await this.form.put('api/places/addresses/' + id)
        .then(({data}) => {
          this.$emit('input', new Address(data));
        });
      } catch(e){

      }
    },

    async delete(id){
      try{
        await this.form.delete('api/places/addresses/' + id);
      } catch(e){

      }
    }
  }
}
</script>
