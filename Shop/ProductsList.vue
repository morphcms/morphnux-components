<template>
  <div>
    <div v-if="grouped" v-for="(chunk, group) in products">
      <h3 class="font-bold text-lg">{{ group || 'Other' }}</h3>
      <list :items="chunk">
        <template v-slot:item="{ item }">
          <product-list-item :product="item">
            <template #description>
              {{ item.hasOwnProperty('summary') ? item.summary : item.description }}
            </template>
            <template #actions>
              <add-to-cart-button :product="item" :unique-per-type="uniquePerType"/>
            </template>
          </product-list-item>
        </template>
      </list>
    </div>

    <list v-else :items="products">
      <template v-slot:item="{ item }">
        <product-list-item :product="item">
          <template #description>
            {{ item.hasOwnProperty('summary') ? item.summary : item.description }}
          </template>
          <template #actions>
            <add-to-cart-button :product="item" :unique-per-type="uniquePerType"/>
          </template>
        </product-list-item>
      </template>
    </list>
  </div>

</template>

<script>
import List from "~/ui/List";
import ProductListItem from '~/components/Shop/ListItem';
import AddToCartButton from "~/components/Shop/AddToCartButton";
import {mapGetters} from "vuex";

export default {
  components: {
    List,
    ProductListItem,
    AddToCartButton,
  },

  props: {
    products: Array | Object,
    uniquePerType: Boolean,
    grouped: Boolean,
  },

  computed: {
    ...mapGetters({
      selectedProducts: 'shop/cart/getProducts',
    }),
  },
}
</script>
