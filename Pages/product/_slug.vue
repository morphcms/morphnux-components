<template>
  <div class="px-40 pt-10 flex flex-col space-y-10">
    <div id="productShowcase" class="grid grid-cols-2 grid-rows-1 max-w-7xl">
      <div id="imageCarousel" class="flex flex-col justify-center items-center space-y-8">
        <img :src="currentImage" alt="featured" class="h-[480px]"/>

        <div v-if="product.gallery.length > 1" class="flex flex-row space-x-4 items-center justify-center overflow-x-auto overflow-y-hidden w-full h-full max-w-7xl">
          <img
            v-for="image in product.gallery"
            :src="image"
            @click="currentImage = image"
            alt="first"
            class="w-36 h-36 rounded-md hover:border hover:border-primary-blue hover:cursor-pointer"
          />
        </div>
      </div>
      <div class="flex flex-col space-y-12">
        <div class="flex flex-col space-y-2">
          <div v-if="product.hasCollections()" class="flex flex-row items-center space-x-4">
            <ui-badge
              v-for="(collection, collectionIdx) in product.collections"
              :key="collectionIdx"
              type="info"
              size="xs"
              :color="collection.color"
              class="uppercase self-start rounded px-8 py-1 text-xs font-bold"
            >
              {{ collection.name }}
            </ui-badge>
          </div>
          <h2 class="text-3xl font-bold text-gray-600">
            {{ product.name }}
          </h2>
          <div class="flex flex-row items-center justify-between">
            <p class="text-primary-blueDarker font-bold">Produs de {{ product.brand }}</p>
          </div>
        </div>

        <div class="flex flex-col space-y-5">
          <product-variation-select
            :variation-slug="product.slug"
            class="flex flex-col space-y-2"
          />

          <div class="flex flex-col space-y-2">
            <h2 class="uppercase font-bold text-xl">Select Size</h2>
            <div class="flex flex-row space-x-2 items-center">
              <ui-badge size="sm"
                        class="bg-white border border-black hover:border-blue-500 hover:bg-blue-200 hover:cursor-pointer hover:border-2">
                20 ml
              </ui-badge>
              <ui-badge size="sm"
                        class="bg-white border border-black hover:border-blue-500 hover:bg-blue-200 hover:cursor-pointer hover:border-2">
                50 ml
              </ui-badge>
              <ui-badge size="sm"
                        class="bg-white border border-black hover:border-blue-500 hover:bg-blue-200 hover:cursor-pointer hover:border-2">
                100 ml
              </ui-badge>
              <ui-badge size="sm"
                        class="bg-white border border-black hover:border-blue-500 hover:bg-blue-200 hover:cursor-pointer hover:border-2">
                1000 cps
              </ui-badge>
            </div>
          </div>
        </div>

        <div class="flex flex-col space-y-5">
          <hr>
          <div class="flex flex-row items-center justify-between">
            <div class="flex flex-col flex-shrink-0">
              <p class="text-4xl font-medium text-gray-600">
                {{ product.defaultPrice().value }}
              </p>
              <p v-show="false" class="text-md font-medium text-gray-700">
                2,34 Lei/tableta
              </p>
            </div>
            <div class="flex flex-row items-center justify-end space-x-3">
              <p class="text-gray-500 text-lg">Quantity</p>
              <ui-button @click.native="product.decrementQty()" size="sm" rounded
                         class="active:bg-primary-blueDarker active:text-white">
                -
              </ui-button>
              <ui-text :value.number="product.quantity" class="w-20 text-center"/>
              <ui-button @click.native="product.incrementQty()" size="sm" rounded
                         class="active:bg-primary-blueDarker active:text-white">
                +
              </ui-button>
            </div>
          </div>

          <ui-button @click.native="addToCart(product)" size="md" color="primary"
                     class="bg-primary-blueDarker justify-center font-bold py-3">
            Add To Cart
          </ui-button>
        </div>
      </div>
    </div>
    <ul id="descriptionNavbar"
         class="grid grid-rows-1 grid-cols-4 text-center divide-x divide-x-4 divide-white bg-primary-blueish w-full rounded-md text-xs">


      <li
        v-for="(tab, tabIdx) in tabs"
        :key="tab + '-' + tabIdx"
        :class="[currentTab === tabIdx ? 'font-bold bg-primary-blueDarker hover:bg-primary-blueDarker text-white' : 'hover:bg-primary-blueDarker hover:text-white']"
        role="tab"
        @click="currentTab = tabIdx"
        class="py-4 hover:text-white hover:cursor-pointer transition">

         {{ tab }}

      </li>
    </ul>

    <div v-show="currentTab === 0" id="descriptionShowcase" >
      <h2 class="mb-2 font-bold">
        Description
      </h2>

      <content-blocks :content="product.content" />
    </div>


    <div v-if="post" id="readMoreAboutIt" class="flex flex-col space-y-7">
      <h2 class="text-3xl font-bold">
        Read more about it:
      </h2>
      <article-card
        :category="post.status"
        :description="post.summary"
        :sub-description="post.summary"
        :image="post.image"
        large
      />
    </div>
    <div v-if="recommended.length > 0" class="flex flex-col space-y-7">
      <h3 class="text-3xl font-bold">
        Recommended for you
      </h3>

      <div class="flex-1 grid grid-cols-4 gap-8 pl-5 max-w-7xl h-full">
        <product-card
          v-for="(product, productIdx) in recommended"
          :key="productIdx"
          :product="product"
        />
      </div>
    </div>
  </div>
</template>

<script>
import ArticleCard from "@/components/Blog/ArticleCard";
import ProductsList from "@/components/Products/ProductsList";
import UiText from "../morphnux/ui/Fields/Text";
import UiButton from "../morphnux/ui/Button";
import UiBadge from "../morphnux/ui/Badge";
import Product from "../morphnux/modules/shop/models/Product";
import {mapActions} from "vuex";

import Post from "../morphnux/modules/blog/models/Post"

import ProductCard from "@/components/Products/ProductCard";
import ProductVariationSelect from "@/components/Products/ProductVariationSelect";
import ContentBlocks from "../morphnux/ui/Blocks/ContentBlocks";
export default {
  name: "product",
  components: {ProductVariationSelect, ContentBlocks, ProductCard, UiBadge, ProductsList, ArticleCard, UiText, UiButton},
  async asyncData({$axios, params}) {

    const {product, post, recommended} = (await $axios.get(`/api/shop/products/${params.slug}`)).data;


    return {
      productPayload: product,
      postPayload: post,
      recommendedPayload: recommended,
    }
  },

  data() {
    return {
      currentImage: null,
      product: {},
      post: null,
      recommended: [],
      currentTab: 0,
      tabs: [
        'Description',
        'Delivery',
        'Nutritional Values',
        'Instructions',
      ]
    }
  },

  created() {
    this.product = new Product(this.productPayload);

    this.currentImage = this.product.image;

    if (this.recommendedPayload) {
      this.recommended = this.recommendedPayload.map((item) => new Product(item));
    }

    if (this.postPayload) {
      this.post = new Post(this.postPayload);
    }

  },

  methods: {
    ...mapActions({
      addToCart: 'shop/cart/add',
      removeFromCart: 'shop/cart/remove',
    }),
  }
}
</script>
