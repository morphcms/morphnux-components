<template>
  <div class="h-full rounded-xl shadow-figma flex flex-col divide-y divide-primary-blueDarker divide-y-2">
    <div class="flex flex-col relative">
      <div class="absolute top-0 left-0 z-10 px-4 pt-2">
        <badge v-if="product.hasDiscount()"
               class="self-start rounded-md text-primary-blueLighter font-bold uppercase text-md py-2 px-4"
               type="info"
               size="customize">

         {{ product.discount.percentage }}
        </badge>
      </div>
     <div class="group relative">
       <div class="w-full min-h-80 bg-gray-200 aspect-w-1 aspect-h-1 rounded-md overflow-hidden group-hover:opacity-75 lg:h-80 lg:aspect-none">
         <img :src="product.image" :alt="product.name" class="w-full h-full object-center object-cover lg:w-full lg:h-full"/>
       </div>
     </div>

    </div>
    <div class="flex-1 flex flex-col justify-between space-y-4 px-4 pt-5 pb-3">
      <div class="flex-1 flex flex-col">
        <nuxt-link :to="'/product/' + product.slug" class="font-bold text-gray-600 text-truncate text-sm">
          {{ product.name }}
        </nuxt-link>
        <p v-if="product.brand" class="text-xs text-gray-400">{{ product.brand }}</p>
      </div>
      <div class="flex flex-row justify-between space-y-1">
        <div class="flex flex-col justify-center">
          <p v-if="product.hasDiscount()" class="text-primary-blueDarker line-through font-bold text-sm">
            {{ product.price.value }}
          </p>
          <p class="text-xl font-bold text-md">
            {{ product.defaultPrice().value }}
          </p>
        </div>

        <add-to-cart-button :product="product"/>
      </div>
    </div>
  </div>
</template>

<script>
import UiButton from "@/ui/Button";
import Badge from "@/ui/Badge";
import Product from "~/modules/shop/models/Product";
import AddToCartButton from "~/components/Shop/AddToCartButton";

export default {
  name: "ProductCard.vue",
  components: {
    AddToCartButton,
    Badge,
    UiButton,
  },
  props: {
    product: Product,
  },
}
</script>
